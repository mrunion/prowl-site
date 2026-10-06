# Build Prowl from source

This guide builds the Prowl Editor from a source checkout. Prowl targets **.NET 10**. You can build and run the editor on Windows, macOS, or Linux with the .NET 10 SDK.

## Prerequisites

- Install the [.NET 10 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/10.0) for your operating system.
- Install Git and clone the [Prowl repository](https://github.com/ProwlEngine/Prowl).
- Have an internet connection for the first restore, which downloads the NuGet dependencies referenced by the solution.

The repository’s solution is `Prowl.slnx`. Use a .NET 10 compatible IDE if you prefer a graphical workflow; the commands below use the .NET CLI and work across all three platforms.

## Build and run the editor

In a terminal, change to the cloned repository root and restore/build the solution:

```sh
dotnet restore Prowl.slnx
dotnet build Prowl.slnx --configuration Debug
```

To build just the editor and its project dependencies, use:

```sh
dotnet build Prowl.Editor/Prowl.Editor.csproj --configuration Debug
```

The editor output is written under `Build/Editor/Debug/`. Run the editor from the repository root with:

```sh
dotnet run --project Prowl.Editor/Prowl.Editor.csproj
```

To open a project immediately and skip the project launcher, pass its folder path:

```sh
dotnet run --project Prowl.Editor/Prowl.Editor.csproj -- --project "/path/to/MyProject"
```

These commands are the same on Windows, macOS, and Linux. On Windows PowerShell, use a quoted Windows path such as `"C:\\Projects\\MyProject"`.

## Build a release configuration

To compile an optimized editor build for the current machine, run:

```sh
dotnet build Prowl.Editor/Prowl.Editor.csproj --configuration Release
```

The output goes under `Build/Editor/Release/`. This is a framework dependent build and may require a compatible .NET runtime on the target computer.

## Publish a self-contained editor

The repository’s release workflow publishes the editor as a self-contained application for these runtime identifiers:

| Platform | Runtime identifier |
| --- | --- |
| Windows, 64-bit x64 | `win-x64` |
| Linux, 64-bit x64 | `linux-x64` |
| Linux, 64-bit ARM | `linux-arm64` |
| macOS, Intel | `osx-x64` |
| macOS, Apple silicon | `osx-arm64` |

Run the matching command from the repository root, replacing `<RID>` with one of the identifiers above:

```sh
dotnet publish Prowl.Editor/Prowl.Editor.csproj \
  --configuration Release \
  --runtime <RID> \
  --self-contained true \
  --output ./publish/<RID>/Prowl
```

For example, to publish for 64-bit Windows:

```sh
dotnet publish Prowl.Editor/Prowl.Editor.csproj \
  --configuration Release \
  --runtime win-x64 \
  --self-contained true \
  --output ./publish/win-x64/Prowl
```

The publish directory contains the editor and its runtime dependencies. You can run the generated executable directly from that directory. Build each target separately; the release workflow produces one package for each runtime identifier.

### macOS application bundle

The release workflow wraps the `osx-x64` and `osx-arm64` publish output in a `Prowl.app` bundle with an `Info.plist`. A plain `dotnet publish` creates the app files but does not perform that bundle step. The workflow’s app bundle is not code signed or notarized, so macOS Gatekeeper may show an unidentified developer warning when launching it.

## Troubleshooting

- **The SDK cannot find .NET 10:** Confirm that the .NET 10 SDK is installed with `dotnet --list-sdks`, then open a new terminal.
- **Restore fails:** Check network access and retry `dotnet restore Prowl.slnx` from the repository root.
- **A native dependency fails to load:** Publish with the runtime identifier that matches both the target operating system and CPU architecture. The editor project includes native libraries for supported platform architectures.
- **You opened the wrong solution:** Use the checked-in `Prowl.slnx` at the repository root.

<!-- Screenshot placeholder: Terminal showing successful restore/build followed by the Prowl Editor launch. Suggested size: 1200 x 700 px (12:7), PNG. -->
