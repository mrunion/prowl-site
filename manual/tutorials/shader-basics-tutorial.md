# Shader Basics: Make a Solid Color Shader

In the [Paint tutorial](material-tutorial.md), you changed the Tint on a Standard material. Now you’ll make a small shader that exposes a color property and outputs that color. This introduces Prowl’s shader file structure, properties, and the fragment stage without requiring a lighting model or textures.

## What you’ll need

You’ll need a Prowl project with a mesh object in a scene. The shader can render a solid color without a light, but a camera is needed to see it in the Game view.

## 1. Create a shader asset

1. In the **Project** panel, open your project’s `Assets` folder.
2. Right-click an empty area and choose **Create > Shader**.
3. Name the file `SolidColor.shader`. The *.shader* extension should be added automatically. Extensions do not show on assets by default but they can be toggled to be displayed from the View settings menu.
4. Open the file in your code editor.

The new shader file starts from a general purpose PBR example, which includes more code than this exercise needs. Replace its contents with the minimal shader below. Save the file; Prowl imports shader files when they change. Check the Console for import or shader compilation errors.

## 2. Add a color property and a pass

Paste this shader into `SolidColor.shader`:

```glsl
Shader "Custom/SolidColor"

Properties
{
    _Color ("Color", Color) = (1.0, 0.2, 0.1, 1.0)
}

Pass "Forward"
{
    Tags { "RenderOrder" = "Opaque" }
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
            layout (location = 0) out vec4 fragColor;
            uniform vec4 _Color;

            void main()
            {
                fragColor = _Color;
            }
        }
    ENDGLSL
}
```

The file has three main parts:

- `Shader "Custom/SolidColor"` gives the shader a name.
- The `Properties` block declares **Color**, with `_Color` as its internal name and a warm red as its starting value. Property names beginning with an underscore are a common convention. The uniform in the fragment stage uses the same `_Color` name.
- The `Forward` pass contains two GLSL stages. The **Vertex** stage includes `ProwlCG` for Prowl's shared shader variables and `VertexAttributes` for mesh inputs and helper functions, then transforms each mesh vertex into clip space. The **Fragment** stage runs for each visible pixel and writes `_Color` to the output.

`RenderOrder = Opaque` places this pass in the opaque rendering queue. `Cull Back` skips back-facing triangles, which is suitable for ordinary closed meshes.

## 3. Create a material with the shader

1. In the Project panel, create a **Material** asset and name it `SolidRed`.
2. Select the material and open its **Shader** picker.
3. Choose **Custom > SolidColor** (the exact menu grouping may show the shader name under its declared path).
4. The material now shows the **Color** property declared in the shader. Change it to any color you like.

The shader supplies the default. The material stores its own override, so two materials can use the same shader and display different colors.

## 4. Assign and view the material

Select your mesh object in the Hierarchy. In its **Mesh Renderer** component, assign `SolidRed` to the material slot by dragging it from the Project panel or using the asset picker. View the result in the Scene or Game view. Since this shader writes its color directly, its output is not shaded by scene lights.

If the material has no Color field, check that the shader imported successfully and that the property name `_Color` is spelled the same in both the `Properties` block and the fragment uniform. If the object disappears, check the Console for a GLSL error and confirm the mesh is facing the camera; `Cull Back` hides its back faces.

## What this shader does not include

This minimal example demonstrates the forward pass and a material color property. It does not provide depth and normals or shadow caster passes, so the object will not contribute correctly to screen-space effects that rely on the prepass or cast shadows through the shader's own shadow pass. A production shader may need additional passes, texture sampling, lighting, transparency handling, and engine vertex helpers. Prowl’s new shader template demonstrates a fuller PBR setup.

## Next steps

Try adding a `Range` property and using it in the fragment calculation, or add a `Texture2D` property and sample it using mesh UVs. For a more complete shader, study the built-in Unlit and Standard shaders and the [Shaders manual](../graphics/shaders.md), which describe properties, passes, tags, and includes.
