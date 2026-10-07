# Prowl for Unity developers

Prowl will feel familiar to Unity developers: both engines organize scenes around GameObjects with Transform data and components, and both use C# scripts attached as behaviors. The Prowl editor also has a Scene view, Game view, Hierarchy, Inspector, Project browser, and Console.

The resemblance is intentional, but it does not make the engines interchangeable. Prowl has its own runtime, editor, serialization, asset pipeline, and scripting API. Treat Unity code and project content as material to port, not as files that can be opened unchanged. Prowl is also in preview, so its APIs and project format may continue to change.

## Concept mapping

| Unity | Prowl | Notes |
| --- | --- | --- |
| GameObject | `GameObject` | Scene object with a Transform and attached components. Both systems support parent-child hierarchies. |
| Component / script component | Component / `MonoBehaviour` | Prowl scripts derive from `Prowl.Runtime.MonoBehaviour`; built-in and custom components appear in the Inspector. Lifecycle details and APIs differ, so review scripts when porting. |
| Scene (`.unity`) | Scene asset (`.scene`) | Both store scene object structures, but the file formats and serialization systems are different. |
| Prefab | Prefab asset | Prowl supports prefab instances, nested prefabs, and tracked overrides, but Unity prefab assets and workflows are not drop-in compatible. |
| Project window | Project browser | Prowl browses project assets under `Assets/`; generated data such as its asset database and thumbnails live under `Library/`. |
| Inspector | Inspector | Displays the selected GameObject’s components and editable properties. Prowl also supports custom component and property editors. |
| Scene view | Scene view | Both provide an editor camera and transform gizmos. Prowl’s Scene view controls are documented in [Scene view](../../editor/scene.md). |
| Game view / Play mode | Game view / Play mode | Prowl runs a separate copy of the current scene while playtesting; edits made to that runtime copy are discarded when play stops. |
| Unity Input System | Prowl Input Actions | Prowl has `.inputactions` assets, action maps, bindings, composites, processors, and action phases. Unity input assets and code still need conversion. |
| Build Profiles / Build Settings | Build Settings and platform profiles | Prowl’s build pipeline is configured in its editor and produces standalone desktop applications. Recreate platform settings and build scripts for Prowl. |

## Differences to account for

- **C# API:** Prowl’s `Prowl.Runtime` API resembles Unity in places but has different types, namespaces, methods, and behavior. Recompile and adapt scripts against Prowl rather than assuming a Unity script will work unchanged.
- **Project and asset format:** A Prowl project has a `.prowl` marker, an `Assets/` folder, and Prowl-specific metadata and settings. Unity project files, scenes, prefabs, and serialized assets are not Prowl project files.
- **Runtime dependency:** Prowl projects compile scripts into project Game and Editor assemblies using .NET tooling. The runtime is designed to run independently of the editor, but Unity-specific packages and editor extensions require alternatives or porting.
- **Preview compatibility:** Prowl’s 1.0-preview project format differs from older Prowl versions and can change before 1.0. Keep source control and backups when evaluating it.

For editor onboarding, start with [Getting Started](../index.md), then see [Prowl’s editor interface](../interface.md) and [scripting guide](../../scripting/index.md). For Unity’s terminology and workflows, use the [Unity Manual](https://docs.unity3d.com/6000.0/Documentation/Manual/index.html), especially its pages on [GameObjects](https://docs.unity3d.com/6000.0/Documentation/Manual/GameObjects.html), [MonoBehaviour](https://docs.unity3d.com/6000.0/Documentation/Manual/class-MonoBehaviour.html), and [input](https://docs.unity3d.com/6000.0/Documentation/Manual/Input.html).
