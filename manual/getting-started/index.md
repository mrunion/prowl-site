# Getting started

Prowl is a C# game engine with a desktop editor. A typical first session is to install the editor, create or open a project, make a scene, and press **Play** to run it in the editor.

This guide introduces that workflow:

1. [Install Prowl](install.md)
2. [Create or open a project](install.md#create-or-open-a-project)
3. [Learn the editor interface](interface.md)

> [!NOTE]
> Prowl is in preview. Project formats and editor workflows can change between releases. The source repository currently identifies its project format as `1.0-preview.5`; use the release that matches the project you intend to open.

## What is a project?

A Prowl project is a folder containing an `Assets` directory, project settings, and a `.prowl` marker file. The editor keeps generated data such as its asset database and thumbnails under `Library`. Store scenes, scripts, and imported assets in the project’s `Assets` folder so they are included in the project workflow.

When you create a project in the launcher, Prowl creates a subfolder named after the project under the location you choose. The initial template is **Blank**. The launcher currently shows other template slots as unavailable.

## Your first editor session

After opening a project, the editor loads a default scene with a camera, directional light, floor, and two cubes. The default layout presents the Scene and Game views, Hierarchy, Inspector, Project browser, and Console. The Hierarchy lists the objects in the open scene. Select an object to inspect its components and properties. Use the Project browser to browse project assets and the Scene view to arrange a scene.

Use **File > New Scene** to start a scene, then **File > Save Scene** or **File > Save Scene As** to save it. Scene files use the `.scene` extension and are stored in the project’s `Assets` folder. Add objects from the Hierarchy context menu or the **GameObject** menu. Select an object and use **Add Component** in the Inspector to attach behavior or other components.

Press **Play** in the editor toolbar to test the current scene in the Game view. Press it again to leave Play mode. Prowl runs a separate copy of the scene during Play mode, so runtime changes are discarded when you stop. Save scene edits outside Play mode.

The following pages cover [installation and project setup](install.md) and the [editor interface](interface.md). Later guides explain specific editor systems such as [input](../input/index.md), [graphics](../graphics/index.md), and [scripting](../scripting/index.md).

<!-- Screenshot placeholder: Prowl Editor showing the default layout and a simple scene. Suggested size: 1600 x 900 px (16:9), PNG. -->
