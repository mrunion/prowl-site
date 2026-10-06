# Install Prowl

Prowl provides prebuilt editor releases and can also be built from source. For most users, download a release from the [Prowl GitHub Releases page](https://github.com/ProwlEngine/Prowl/releases). Check the release notes and included files for the package and platform-specific launch instructions; the exact release packaging can change during preview.

## Requirements

The checked-in editor and runtime target **.NET 10**. For a source build, install the .NET 10 SDK. For a prebuilt release, follow its release notes to determine whether it includes the runtime or requires a separate .NET 10 installation. You can install .NET from the [official .NET download page](https://dotnet.microsoft.com/en-us/download/dotnet/10.0).

The editor is developed for Windows, macOS, and Linux. Confirm that the selected release includes a build for your operating system and processor architecture.

## Create or open a project

1. Start **Prowl Editor**. The project launcher opens before the editor.
2. To create a project, select **New Project**.
3. Select the **Blank** template. It starts with an empty project structure; additional template cards may be unavailable.
4. Enter a project name and parent location. By default, Prowl uses `Documents/Prowl Projects`.
5. Select **Create Project**. Prowl creates a folder named after the project inside the selected parent location and opens it. For example, the name `My Game` and location `Documents/Prowl Projects` create `Documents/Prowl Projects/My Game`.

To open an existing project, choose it from the **Recent** tab, or use **Open Project** to browse to its project folder. A valid project contains an `Assets` folder. You can also pass a project folder or `.prowl` marker file to Prowl using the `--project` command-line option.

Prowl checks the project’s saved engine version when it opens. If the project needs migration, the launcher asks before applying the migration. Projects created by newer engine versions cannot be opened by an older editor. Make a backup before accepting a migration, especially while Prowl is in preview.

## Project folders

A newly created project includes these key locations:

| Location | Purpose |
| --- | --- |
| `Assets/` | Scenes, scripts, and other project assets. |
| `ProjectSettings/` | Project configuration. |
| `Packages/` | Package-related project data. |
| `Library/` | Generated editor data, including the asset database, thumbnails, and editor state. |
| `Temp/`, `Logs/`, `Backups/` | Temporary files, logs, and backups. |
| `<ProjectName>.prowl` | Project marker and saved engine version information. |

Prowl also creates a `.gitignore` that excludes generated folders and generated project/solution files, and a `Directory.Build.props` file for project-level MSBuild customization such as additional NuGet references.

## Build from source

To build the editor yourself, install the .NET 10 SDK, clone the [Prowl repository](https://github.com/ProwlEngine/Prowl), then build `Prowl.slnx` with the .NET CLI or an IDE that supports .NET 10. The repository README has the current source-build notes. Source builds can require additional platform tooling and dependencies.

<!-- Screenshot placeholder: Project launcher with Recent and New Project tabs visible. Suggested size: 1400 x 900 px (7:4), PNG. -->
