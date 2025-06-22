# Player Extensions

The `NetPlayer` class can be extended using the `IPlayerExtension` and `IPlayerExtensionBuilder<T>` interfaces.

## Implementing an Extension
Example implementation of `IPlayerExtension`:
```cs
public sealed class MyPlayerExtension : IPlayerExtension
{
    internal MyPlayerExtension(NetPlayer plr)
        => Player = plr;

    public NetPlayer Player { get; }

    // Test fields
    public bool Field1; 
    public int Field2;

    public void Load()
    {
        // Initialize the extension for the player
        AmethystLog.Main.Info("MyPlayerExtension", $"MyPlayerExtension loaded for {Player.Name}!");
    }

    public void Unload()
    {
        // Deinitialize the extension for the player
        AmethystLog.Main.Error("MyPlayerExtension", $"MyPlayerExtension unloaded for {Player.Name}!");
    }
}
```

## Implementing a Builder
Example implementation of `IPlayerExtensionBuilder<T>`:
```cs
public sealed class MyExtensionBuilder : IPlayerExtensionBuilder<MyPlayerExtension>
{
    public void Initialize()
    {
        // Extension initialization
        AmethystLog.Main.Info("MyPlayerExtension", $"MyExtensionBuilder initialized!");
    }

    public MyPlayerExtension Build(NetPlayer player)
        => new MyPlayerExtension(player);
}
```

## Registering the Extension
Register the extension in a plugin:
```cs
public sealed class MyPlugin : PluginInstance
{
    public override string Name => "MyPlugin";
    public override Version Version => new Version(1, 0);

    protected override void Load()
    {
        PlayerExtensions.RegisterBuilder<MyPlayerExtension>(new MyExtensionBuilder());
    }

    protected override void Unload()
    {
        PlayerExtensions.UnregisterBuilder<MyPlayerExtension>();
    }
}
```

# Retrieving Extensions
After registration, retrieve the extension from `NetPlayer`:
```cs
NetPlayer player;
MyPlayerExtension? ext = player.GetExtension<MyPlayerExtension>();

if (ext is null)
    AmethystLog.Main.Error("MyPlayerExtension", "Extension not initialized!");
else
    AmethystLog.Main.Info("MyPlayerExtension", "Extension retrieved successfully!");
```

# Manual Reload (Rarely Needed)
```cs
NetPlayer player;

// Unload extension
player.UnloadExtension<MyPlayerExtension>();

// Reload extension
player.LoadExtension<MyPlayerExtension>();
```