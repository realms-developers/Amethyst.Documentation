# Module Development and Management

First, understand what a module is.

A module is a static extension for the server, designed to expand API functionality.

Modules are loaded from the `extensions/modules` directory, provided they are allowed (via the `modules setallow` command below).

## Managing Modules

After starting the server, use these console commands:

- `modules list` - Lists loaded modules.
- `modules allowlist` - Lists allowed modules.
- `modules setallow` `<name with .dll> <true | false>` - Allow or block a module.
  - Example: `modules setallow MyModule.dll true`

## Creating a Module
Install the `Amethyst.Templates` NuGet package first:
```bash
dotnet new install Amethyst.Templates
```

The `Amethyst.Templates` package simplifies creating plugins/modules for the API.

Create a module named `MyModule`:
```bash
dotnet new aext-module -n MyModule
```

After running the command, a `MyModule` folder or project will be created.

### Base Project Code
```cs
using Amethyst.Extensions.Modules;

namespace MyModule;

[AmethystModule("MyModule")]
public static class MyModule
{
    private static bool IsInitialized;
    [ModuleInitialize]
    public static void Initialize()
    {
        if (IsInitialized) return;
        IsInitialized = true;

        // Module initialization code
    }
}
```

Compile the module:
```bash
dotnet build -c Release
```

The compiled module (e.g., `MyModule.dll`) will be in `bin/Release/net7.0`. Move it to `extensions/modules` in the Amethyst root folder.

Start the server, run `/modules setallow MyModule.dll`, and reload the server. Your first module is now running!

### Dependencies
To use third-party libraries, specify dependencies in the module attributes:

For a library at `deps/MyEpicLibrary.dll`:
```cs
using Amethyst.Extensions.Modules;

namespace MyModule;

[AmethystModule("MyModule", "deps/MyEpicLibrary.dll")]
public static class MyModule
...
```

Multiple dependencies:
```cs
[AmethystModule("MyModule", "deps/Library1.dll", "deps/Library2.dll", "deps/Library3.dll")]
public static class MyModule
...
```