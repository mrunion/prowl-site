# Shader Advanced: Make a Lit Paint Material

In [Shader Basics](shader-basics-tutorial.md), you created an unlit shader that outputs a color. Here you’ll build on that idea to make a solid-color paint material that responds to scene lights, receives shadows, and casts shadows. The shader uses Prowl’s PBR lighting helpers and includes the forward, prepass, and shadow caster passes expected of an opaque game material. It has no emission or texture inputs.

## What you’ll need

You’ll need a Prowl project with a mesh object and a camera. Add at least one light to the scene and enable its shadow casting to see the material respond to lighting and shadows. A second mesh or a floor under the object makes its cast shadow easier to see.

## 1. Create a shader asset

1. In the **Project** panel, open your project’s `Assets` folder.
2. Right-click an empty area and choose **Create > Shader**.
3. Name the file `Paint.shader`. The *.shader* extension should be added automatically. Extensions do noy show on assets by default but they can be toggled to be displayed from the View settings menu.
4. Open the file in your code editor. Replace its contents with the shader below and save it. Prowl imports shader files when they change; check the Console for import or compilation errors.

## 2. Add a color property and lit passes

Paste this into `Paint.shader`:

```glsl
Shader "Custom/Paint"

Properties
{
    _MainColor ("Paint Color", Color) = (0.65, 0.12, 0.08, 1.0)
    _Metallic ("Metallic", Range(0.0, 1.0)) = 0.0
    _Roughness ("Roughness", Range(0.0, 1.0)) = 0.35
}

// Forward color pass: evaluate direct and ambient PBR lighting.
Pass "Forward"
{
    Tags { "RenderOrder" = "Opaque" }
    Cull Back
    GLSLPROGRAM

        Vertex
        {
            #include "ProwlCG"
            #include "VertexAttributes"

            out vec3 worldPos;
            out vec3 worldNormal;

            void main()
            {
                gl_Position = TransformClip(vertexPosition);
                worldPos = TransformPosition(vertexPosition);
                worldNormal = TransformDirection(vertexNormal);
            }
        }

        Fragment
        {
            #include "ProwlCG"
            #include "Lighting"

            layout (location = 0) out vec4 fragColor;

            in vec3 worldPos;
            in vec3 worldNormal;

            uniform vec4 _MainColor;
            uniform float _Metallic;
            uniform float _Roughness;

            void main()
            {
                vec3 normal = normalize(worldNormal);
                vec3 viewDir = normalize(_WorldSpaceCameraPos.xyz - worldPos);
                vec3 albedo = gammaToLinearSpace(_MainColor.rgb);
                float metallic = clamp(_Metallic, 0.0, 1.0);
                float roughness = clamp(_Roughness, 0.0, 1.0);

                vec3 direct = CalculateForwardLighting(worldPos, normal, viewDir,
                                                       albedo, metallic, roughness, 1.0,
                                                       0.0, 0.0, 0.5, 1.0);
                vec3 ambient = CalculateAmbient(normal) * albedo * _AmbientStrength;
                vec3 color = ApplyFog(ambient + direct, worldPos);
                fragColor = vec4(color, _MainColor.a);
            }
        }
    ENDGLSL
}

// Depth, normals, and surface response for screen-space effects.
Pass "Prepass"
{
    Tags { "LightMode" = "Prepass" }
    Cull Back
    ZWrite On
    GLSLPROGRAM

        Vertex
        {
            #include "ProwlCG"
            #include "VertexAttributes"

            out vec3 worldNormal;
            out vec4 vCurrClipNJ;
            out vec4 vPrevClip;

            void main()
            {
                gl_Position = TransformClip(vertexPosition);
                worldNormal = TransformDirection(vertexNormal);

                // Motion vectors must use non-jittered clip positions.
                vec4 motionWorldPos = GetModelMatrix() * vec4(vertexPosition, 1.0);
                vCurrClipNJ = PROWL_MATRIX_VP_NONJITTERED * motionWorldPos;
                vec4 prevWorldPos = PROWL_MATRIX_M_PREVIOUS * vec4(vertexPosition, 1.0);
                vPrevClip = PROWL_MATRIX_VP_PREVIOUS * prevWorldPos;
            }
        }

        Fragment
        {
            #include "ProwlCG"

            layout (location = 0) out vec4 normalOut;
            layout (location = 1) out vec4 motionRM;

            in vec3 worldNormal;
            in vec4 vCurrClipNJ;
            in vec4 vPrevClip;

            uniform float _Metallic;
            uniform float _Roughness;

            void main()
            {
                normalOut = EncodeViewNormal(normalize(worldNormal));
                vec2 currNDC = (vCurrClipNJ.xy / vCurrClipNJ.w) * 0.5 + 0.5;
                vec2 prevNDC = (vPrevClip.xy / vPrevClip.w) * 0.5 + 0.5;
                motionRM = vec4(currNDC - prevNDC,
                                clamp(_Roughness, 0.0, 1.0),
                                clamp(_Metallic, 0.0, 1.0));
            }
        }
    ENDGLSL
}

// Depth-only pass used when this material casts a shadow.
Pass "ShadowCaster"
{
    Tags { "LightMode" = "ShadowCaster" }
    Cull Back
    GLSLPROGRAM

        Vertex
        {
            #include "ProwlCG"
            #include "VertexAttributes"

            void main()
            {
                gl_Position = TransformClip(vertexPosition);
            }
        }

        Fragment
        {
            #include "ProwlCG"

            void main()
            {
                gl_FragDepth = gl_FragCoord.z;
            }
        }
    ENDGLSL
}
```

The **Forward** pass transforms the mesh position and normal into world space, converts the selected paint color from sRGB to linear space, and evaluates PBR lighting. `CalculateForwardLighting` accounts for direct lights and their shadows; `CalculateAmbient` adds ambient environment light; `ApplyFog` applies scene fog. The Metallic and Roughness controls shape the surface response. A low metallic value and moderate roughness make a useful starting point for ordinary painted surfaces.

The **Prepass** writes a view-space normal and the roughness and metallic values for screen-space effects such as ambient occlusion and reflections. Since this shader uses a constant color and no textures, it does not need texture coordinates or tangent data.

The **ShadowCaster** pass renders the mesh into shadow maps. It writes depth rather than the paint color; the forward lighting helper uses the scene shadow maps when shading the visible surface.

## 3. Create a material with the shader

1. In the Project panel, create a **Material** asset and name it `RedPaint`.
2. Select the material and open its **Shader** picker.
3. Choose **Custom > Paint** (the exact menu grouping may show the declared shader path).
4. Set **Paint Color**, **Metallic**, and **Roughness**. The shader defaults to a red, non-metallic surface with moderate gloss.

The material stores its property values, so you can create other paint colors or finishes from the same shader.

## 4. Assign it and check the lighting

1. Select your mesh object in the Hierarchy.
2. In its **Mesh Renderer** component, assign `RedPaint` to the material slot.
3. Make sure a light illuminates the object. Enable **Cast Shadows** on the light, and enable shadow casting for the object if its renderer exposes that setting.
4. Place another object or a floor where the mesh’s shadow can fall. View the result in the Scene or Game view.

If the material appears black, add or reposition a light and check the scene’s ambient lighting. If it does not cast a shadow, confirm both the light and renderer cast shadows and that the shader imported without errors. If normal-based effects look wrong, confirm the mesh has valid normals.

## Next steps

Try adding an albedo texture and UVs, or add a normal map with tangent data. For the full set of PBR helpers and pass conventions, see the [Shaders manual](../graphics/shaders.md) and compare the built-in [Standard shader](../graphics/materials.md).
