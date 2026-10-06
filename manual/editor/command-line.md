# Command line

The Prowl Editor accepts command-line options for opening a project directly or building a project without opening the editor interface. Run the editor executable from a terminal or automation script. The option names are case-sensitive.

When building from a source checkout, invoke the editor through `dotnet run` from the repository root:

```sh
dotnet run --project Prowl.Editor/Prowl.Editor.csproj -- --help
```

The `--` separates `dotnet run` options from arguments passed to Prowl. For a packaged editor, run its executable directly and omit the `dotnet run --project ... --` prefix.

## Options

| Option | Aliases | Argument | Description |
| --- | --- | --- | --- |
| `--help` | `-help`, `-h` | Optional option name | Displays the available commands, or details for one option. |
| `--project` | `-project`, `-p` | Project folder or `.prowl` file | Opens the project directly and skips the launcher. |
| `--buildmode` | `-build`, `-b` | Project folder or `.prowl` file | Builds the project without opening the editor interface. |
| `--output` | `-output`, `-o` | Directory path | Overrides the output directory for the headless build. |

## Open a project

Pass `--project` followed by the project folder or its `.prowl` marker file:

```sh
Prowl.Editor --project "/path/to/MyProject"
```

From a source checkout:

```sh
dotnet run --project Prowl.Editor/Prowl.Editor.csproj -- --project "/path/to/MyProject"
```

Prowl loads the project and skips the project launcher. A project opened this way still opens the editor UI.

## Build without the editor UI

Use `--buildmode` with a project folder or `.prowl` file to start the configured build pipeline without opening the editor window:

```sh
Prowl.Editor --buildmode "/path/to/MyProject"
```

By default, the output directory is `Builds` beside the project folder (the equivalent of `<project folder>/../Builds`). Set a different output directory with `--output`:

```sh
Prowl.Editor --buildmode "/path/to/MyProject" --output "/path/to/build-output"
```

From a source checkout:

```sh
dotnet run --project Prowl.Editor/Prowl.Editor.csproj -- \
  --buildmode "/path/to/MyProject" \
  --output "/path/to/build-output"
```

The headless build uses the project’s saved Build settings, including the selected pipeline, target profile, configuration, scenes, and asset packaging options. Configure these in the editor before building. Prowl writes build progress and results to standard output; messages are encoded for the editor’s build status UI, so command-line output is intended primarily for build automation and logs.

The current implementation starts the build and exits after starting it; it does not wait for completion in `Main`. When scripting builds, verify that the output was produced and inspect the reported result instead of treating process launch alone as proof of success.

## Create a project

There is no command-line project creation option in the current editor. Create projects from the project launcher using **New Project**, or create one in the editor and then use `--project` to open it.

## Help

Show all options:

```sh
Prowl.Editor --help
```

Show details for one option by passing it after the help option:

```sh
Prowl.Editor --help --buildmode
```

For source builds, substitute the `dotnet run --project ... --` prefix shown above.
