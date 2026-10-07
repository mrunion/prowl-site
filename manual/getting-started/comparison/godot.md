# Prowl for Godot developers

Godot and Prowl both let you build games by composing objects into scenes, attach scripts or other behavior, and test from an editor. The main difference is the structure of that composition: Godot scenes are trees of typed Nodes, while Prowl scenes contain GameObjects with attached components. A Prowl GameObject is a familiar starting point if you think in entities and components, but it does not map one-to-one to every Godot Node type.

Godot supports GDScript and C# (through its .NET editor build). Prowl uses C# and .NET for its editor scripting workflow. Godot’s signals, resources, node lifecycle, and scene instancing have their own design; Prowl offers comparable engine features through different APIs and serialization. Port gameplay concepts, then adapt the code and content to the target engine.

## Concept mapping

| Godot | Prowl | Notes |
| --- | --- | --- |
| Node | GameObject or component | A Godot Node is a typed unit in a scene tree. In Prowl, a GameObject is a container for components; the closest mapping depends on the Node’s role. For example, visual, camera, and physics Nodes often correspond to GameObjects with renderer, Camera, or collider components. |
| Scene tree | GameObject hierarchy | Both organize parent-child relationships. Prowl’s Hierarchy shows GameObjects in the current scene. |
| Scene (`.tscn` / `.scn`) | Scene asset (`.scene`) | Both can represent reusable scene structures, but the formats and resource systems are different. |
| PackedScene instance | Prefab instance or instantiated scene | Godot commonly saves reusable node trees as PackedScenes. Prowl has prefabs and scene assets, with different instancing and override behavior. |
| Script attached to a Node | `MonoBehaviour` attached to a GameObject | Prowl behavior scripts derive from `Prowl.Runtime.MonoBehaviour`. Godot scripts can use GDScript or C# and extend a specific Node class. |
| Signal | Event or callback | Godot signals connect objects through an event system. Prowl uses C# events and engine-specific callbacks/APIs; there is no automatic signal or script conversion. |
| Resource | Prowl asset | Both engines use reusable data and asset objects. Prowl imports files into its own asset database and serialized asset model. |
| Input Map / actions | Prowl Input Actions | Prowl uses `.inputactions` assets with action maps, bindings, composites, processors, and phases. Recreate input actions and bindings in the new project. |
| 2D / 3D editor views | Scene and Game views | Prowl’s Scene view is an editable 3D viewport; the Game view previews through a game camera. Check Prowl’s current feature set before assuming a Godot editor tool has a direct counterpart. |

## Differences to account for

- **Node composition vs. components:** Godot often represents behavior and capabilities as different Node types in the scene tree. Prowl tends to keep the GameObject as the tree node and attach capabilities as components shown together in the Inspector. When porting, decide which Godot nodes become separate GameObjects and which become components.
- **Lifecycle and API:** Godot uses virtual callbacks such as `_Ready()` and `_Process()` on Nodes. Prowl has its own `MonoBehaviour` lifecycle and scene dispatch. Translate behavior deliberately; callback names and timing are not guaranteed to match.
- **Language and tooling:** Godot supports GDScript and C#, with C# projects using Godot’s .NET editor build and .NET SDK. Prowl’s editor scripting uses C# and compiles project scripts with .NET tooling. GDScript must be rewritten in C# for Prowl.
- **Content compatibility:** Godot scenes, resources, scripts, and project settings cannot be opened directly as Prowl assets. Recreate or convert content and verify resource references in the Prowl editor.
- **Preview status:** Prowl’s APIs and project format are still evolving. Keep the source project under version control and check the release notes when updating.

For a first look at the editor, start with [Getting Started](../index.md) and the [editor interface](../interface.md). The [Prowl Scene view guide](../../editor/scene.md) covers viewport navigation and transforms. For Godot’s concepts and workflow, see the [Godot documentation](https://docs.godotengine.org/en/stable/), particularly [Nodes and scene instances](https://docs.godotengine.org/en/stable/tutorials/scripting/nodes_and_scene_instances.html), [GDScript and C#](https://docs.godotengine.org/en/stable/getting_started/introduction/learn_to_code_with_gdscript.html), and [signals](https://docs.godotengine.org/en/stable/getting_started/step_by_step/signals.html).
