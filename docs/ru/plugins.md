# Разработка, управление плагинами

Для начала стоит понять, что такое плагин.

Плагин - отдельное расширение для сервера, которое обязуется быть загружаемым и выгружаемым. Оно обязано не предоставлять API, а лишь взаимодействовать с установленными.

Плагины загружаются из директории `extensions/plugins`, при том условии что вы разрешили загрузку. (команда `plugins setallow` ниже)

## Управление плагинами

После запуска сервера, вы можете вводить команды в консоль:

- `plugins list` - показывает загруженные плагины.
- `plugins allowlist` - показывает разрешенные плагины.
- `plugins setallow` `<название с .dll> <true | false>` - разрешает плагин или наоборот.
    - Пример: `plugins setallow MyPlugin.dll true`
- `plugins load` - загружает плагины.
- `plugins unload` - выгружает плагины.
- `plugins reload` - перезагружает плагины.

## Создание плагина
Для начала рекомендуется установить NuGet-пакет `Amethyst.Templates`.

```bash
dotnet new install Amethyst.Templates
```

Пакет `Amethyst.Templates` позволяет удобно и быстро создавать плагины/модули для API.

Создадим первый плагин, назовем его MyPlugin:
```bash
dotnet new aext-plugin -n MyPlugin
```

После выполнения команды, очевидно появится папка MyPlugin или весь проект будет в вашей папке.

### Базовый исходный код проекта

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
        // Здесь выполняется код загрузки плагина.

        AmethystLog.Startup.Info("Amethyst.Docs", "MyPlugin.Load();");
    }

    protected override void Unload()
    {
        // Здесь выполняется код вызагрузки плагина.

        AmethystLog.Startup.Info("Amethyst.Docs", "MyPlugin.Unload();");
    }
}
```

После того как вы создали плагин, его нужно скомпилировать: `dotnet build -c Release`.

В папке `bin/Release/net9.0` будет скомпилированный плагин. В нашем случае `MyPlugin.dll`

Переместите его в папку `extensions/plugins` в корневой папке Amethyst.

Затем запустите сервер, и введите команду `/plugins setallow MyPlugin.dll`, а затем перезагрузите плагины командой `/plugins reload`.

Вот и все, первый ваш плагин уже работает!