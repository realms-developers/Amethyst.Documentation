# Debug Mode

Debug mode (or developer mode) enables the following features:

- The `/grantroot` command, which grants root mode to a player. Root mode gives a player all permissions.
- Game update debugging.
- Enables debug logs.
- Displays reasons for ignored packets in the console.

To start the server in debug mode, add the `-debug` argument to the launch command (`start.bat` or `./start.sh`).

# Usage in Plugins

You can also utilize this state in plugins/modules.

Check if the server is in debug mode via `AmethystSession.Profile.DebugMode`.

Example:

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
        // This log will only appear if debug mode is enabled, as debug logs are active only in debug mode.
        AmethystLog.Startup.Debug("Amethyst.Docs", "Debug message from my epic plugin!");

        if (AmethystSession.Profile.DebugMode)
        {
            AmethystLog.Startup.Debug("Amethyst.Docs", "This message will definitely appear now.");
        }
        AmethystLog.Startup.Info("Amethyst.Docs", "This message will also appear");
    }

    protected override void Unload() {}
}
```