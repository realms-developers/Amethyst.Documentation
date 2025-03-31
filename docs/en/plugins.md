# Plugin Development and Management

First, understand what a plugin is.

A plugin is a dynamic extension for the server, designed to interact with existing APIs without exposing new ones.

Plugins are loaded from the `extensions/plugins` directory, provided they are allowed (via the `plugins setallow` command below).

## Managing Plugins

After starting the server, use these console commands:
- `plugins list` - Lists loaded plugins.
- `plugins allowlist` - Lists allowed plugins.
- `plugins setallow` `<name with .dll> <true | false>` - Allow or block a plugin.
  - Example: `plugins setallow MyPlugin.dll true`
- `plugins load` - Load plugins.
- `plugins unload` - Unload plugins.
- `plugins reload` - Reload plugins.

## Creating a Plugin
Install the `Amethyst.Templates` NuGet package first:
```bash
dotnet new install Amethyst.Templates
```

Create a plugin named `MyPlugin`:
```bash
dotnet new aext-plugin -n MyPlugin
```

### Base Project Code
```cs
using Amethyst.Extensions.Plugins;

using System;

namespace MyPlugin;

public sealed class MyPlugin : PluginInstance
{
    public override string Name => "MyPlugin";

    public override Version Version => new Version(1, 0);

    protected override void Load()
    {
        // Plugin loading code

        AmethystLog.Startup.Info("Amethyst.Docs", "MyPlugin.Load();");
    }

    protected override void Unload()
    {
        // Plugin unloading code

        AmethystLog.Startup.Info("Amethyst.Docs", "MyPlugin.Unload();");
    }
}
```

Compile the plugin:
```bash
dotnet build -c Release
```

The compiled plugin (e.g., `MyPlugin.dll`) will be in `bin/Release/net7.0`. Move it to `extensions/plugins` in the Amethyst root folder.

Start the server, run `/plugins setallow MyPlugin.dll`, and reload plugins with `/plugins reload`. Your first plugin is now active!