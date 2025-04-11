# Режим отладки

Режим отладки (или режим разработчика) включает некоторые функции:

- команда `/grantroot`, которая переключает root-режим игроку. Root-режим - режим при котором игрок имеет все права.
- отладка обновления игры.
- включает Debug-логи.
- показывает в консоли, почему пакеты игнорируются.

Чтобы запустить сервер в режиме отладки, добавьте аргумент `-debug` к запуску (`start.bat` или `./start.sh`)

# Использование в плагинах

Вы также можете использовать это состояние в плагинах/модулях.

Понять, что сервер находится в режиме отладки, можно через `AmethystSession.Profile.DebugMode`.

Например:

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
        // лог высветится только при включенном режиме отладки, так как Debug-логи работают только в режиме Debug.
        AmethystLog.Startup.Debug("Amethyst.Docs", "Debug message from my epic plugin!");

        if (AmethystSession.Profile.DebugMode)
        {
            AmethystLog.Startup.Debug("Amethyst.Docs", "А вот теперь это сообщение точно отобразится.");
        }
        AmethystLog.Startup.Info("Amethyst.Docs", "А это сообщение - темболее");
    }

    protected override void Unload() {}
}
```