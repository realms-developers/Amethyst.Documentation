# Разработка, управление модулями

Для начала стоит понять, что такое модуль.

Модуль - отдельное расширение для сервера, которое обязуется быть статическим и по возможности расширять функционал API.

Модули загружаются из директории `extensions/modules`, при том условии что вы разрешили загрузку. (команда `modules setallow` ниже)

## Управление модулями

После запуска сервера, вы можете вводить команды в консоль:

| Команда                                              | Описание                                                                   | Пример                                   |
|------------------------------------------------------|----------------------------------------------------------------------------|------------------------------------------|
| `modules list`                                       | Показывает загруженные модули.                                             |                                          |
| `modules allowlist`                                  | Показывает разрешенные модули.                                             |                                          |
| `modules setallow <название с .dll> <true \| false>` | Разрешает или блокирует модуль.                                            | `modules setallow MyModule.dll true`     |

## Создание модуля
Для начала рекомендуется установить NuGet-пакет `Amethyst.Templates`.

```bash
dotnet new install Amethyst.Templates
```

Пакет `Amethyst.Templates` позволяет удобно и быстро создавать плагины/модули для API.

Создадим первый модуль, назовем его MyModule:
```bash
dotnet new aext-module -n MyModule
```

После выполнения команды, очевидно появится папка MyModule или весь проект будет в вашей папке.

### Базовый исходный код проекта

```cs
using Amethyst.Extensions.Modules;

namespace MyModule;

[AmethystModule("MyModule")]
public static class MyModule
{
    private static bool _isInitialized;
    
    [ModuleInitialize]
    public static void Initialize()
    {
        if (IsInitialized)
        {
            return;
        }
        
        IsInitialized = true;

        // код инициализации модуля . . .
    }
}
```

После того как вы создали модуль, его нужно скомпилировать: `dotnet build -c Release`.

В папке `bin/Release/net9.0` будет скомпилированный модуль. В нашем случае `MyModule.dll`

Переместите его в папку `extensions/modules` в корневой папке Amethyst.

Затем запустите сервер, и введите команду `/modules setallow MyModule.dll`, а затем перезагрузите сервер.

Вот и все, первый ваш модуль уже работает!