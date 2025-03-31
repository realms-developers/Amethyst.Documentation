# Дополнения

Класс `NetPlayer` предоставляет возможность расширять его, с помощью интерфейсов `IPlayerExtension`

Чтобы создать расширение игроку, нужно унаследовать два класса: `IPlayerExtension` и `IPlayerExtensionBuilder<T>` 

Например, имплементация для `IPlayerExtension` выглядит так:

```cs
public sealed class MyPlayerExtension : IPlayerExtension
{
    internal MyPlayerExtension(NetPlayer plr)
        => Player = plr;

    public NetPlayer Player { get; }

    // тестовые поля
    public bool Field1; 
    public int Field2;

    public void Load()
    {
        // Инициализация расширения для игрока.

        AmethystLog.Main.Info("MyPlayerExtension", $"MyPlayerExtension was loaded for {Player.Name}!");
    }

    public void Unload()
    {
        // Деинициализация расширения для игрока.

        AmethystLog.Main.Error("MyPlayerExtension", $"MyPlayerExtension was unloaded for {Player.Name}!");
    }
}
```

Соответственно, для `IPlayerExtensionBuilder<T>`:

```cs
public sealed class MyExtensionBuilder : IPlayerExtensionBuilder<MyPlayerExtension>
{
    public void Initialize()
    {
        // Инициализация расширения.

        AmethystLog.Main.Info("MyPlayerExtension", $"MyExtensionBuilder for MyPlayerExtension was initialized!");
    }

    public MyPlayerExtension Build(NetPlayer player)
        => new MyPlayerExtension(player);
}
```

С структурой расширений все понятно, но теперь нужно инициализировать расширение:

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

# Получение экземпляра IPlayerExtension от NetPlayer.

После того, как вы добавили инициализацию расширения, его можно использовать в `NetPlayer`

Допустим:

```cs
NetPlayer player;
MyPlayerExtension? ext = player.GetExtension<MyPlayerExtension>();

// если ext является null, значит вы ничего не инициализировали.

if (ext == null)
    AmethystLog.Main.Error("MyPlayerExtension", $"Me when amogus is sus!");
else
    AmethystLog.Main.Info("MyPlayerExtension", $"its not null omg");

```

Также его можно выгружать и загружать (в большинстве случаев не нужно):

```cs
NetPlayer player;

player.UnloadExtension<MyPlayerExtension>();

player.LoadExtension<MyPlayerExtension>();
```