# Editor interface

The Prowl editor is made of dockable panels. You can resize panels by dragging their dividers and move panels by dragging their tabs. The default layout puts the Scene and Game views in the main area, the Project browser and Console below them, and the Hierarchy and Inspector on the right. Prowl saves the panel layout between sessions; choose **Edit > Save Layout** to save it explicitly.

<!-- Screenshot placeholder: Annotated default editor layout labeling the menu bar, toolbar, Scene view, Game view, Hierarchy, Inspector, Project browser, Console, and status bar. Suggested size: 1920 x 1080 px (16:9), PNG. -->

## Main panels

| Panel | What it shows and how you use it |
| --- | --- |
| **Scene** | An editable view of the current scene. Select and arrange objects here. The Scene view has camera navigation controls and transform gizmos. |
| **Game** | The view rendered by the scene’s active camera. Use it to preview the game, especially while in Play mode. |
| **Hierarchy** | The GameObjects in the current scene, shown in parent-child order. Select, create, rename, duplicate, or organize scene objects here. |
| **Inspector** | Components and editable properties for the current selection. Use **Add Component** to attach a component to a selected GameObject. |
| **Project** | Files and assets in the project. Drag assets into the Scene view or onto compatible asset fields in the Inspector. Right-click empty space to create assets. |
| **Console** | Editor and runtime log messages, including warnings and errors. Use it to diagnose script compilation and playtest issues. |

Open panels from the **Window** menu if one is closed. Panels can be docked, resized, or floated to suit your workspace.

## Scene view navigation and tools

When the Scene view is focused:

- Hold **Alt** and drag with the left mouse button to orbit the view.
- Hold **Alt** and drag with the right mouse button to dolly the view.
- Drag with the middle mouse button to pan.
- Hold the right mouse button and use **W**, **A**, **S**, and **D** to fly the camera. Hold **Shift** to move faster. Use **E** or **Space** to rise and **Q** to descend; scroll the mouse wheel to adjust flight speed.
- Select a GameObject and press **F** to focus the Scene camera on it.
- Click the orientation cube to snap to an axis-aligned view.

The transform tools are **W** (move), **E** (rotate), **R** (scale), and **T** (combined transform). Hold **Ctrl** while dragging a gizmo to use snapping.

<!-- Screenshot placeholder: Scene view with a selected object, transform gizmo, and orientation cube. Suggested size: 1200 x 750 px (8:5), PNG. -->

## Menus and common actions

- **File** contains scene creation, opening, saving, and project build commands.
- **Edit** contains Undo, Redo, Project Settings, Save Layout, and Preferences.
- **Assets** contains asset tools, including refresh, reimport, and package import.
- **GameObject** contains commands to create and organize scene objects.
- **Window** opens editor panels.

Some commands depend on the current selection or project. Right-click the Hierarchy, Project panel, or a component header for context actions. Use **Ctrl+Z** to undo and **Ctrl+Y** to redo. Press **F2** to rename a selected object or asset, and **Ctrl+D** to duplicate a selection.

## Play mode

Select **Play** in the toolbar to run the current scene in the Game view. Prowl sends gameplay input when the Game view has focus. If the game locks the cursor, press **Escape** to release it. Select **Play** again to stop.

Play mode is for testing. Prowl runs a separate copy of the scene, and changes made to that copy are discarded when you stop. Make lasting scene edits outside Play mode.

## Status bar

The status bar provides quick access to recent Console messages and severity counts for information, warnings, and errors. Click the status area to open the Console panel.
