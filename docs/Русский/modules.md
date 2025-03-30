# Разработка, управление модулями

Для начала стоит понять, что такое модуль.

Модуль - отдельное расширение для сервера, которое обязуется быть статическим и по возможности расширять функционал API.

Модули загружаются из директории `extensions/modules`, при том условии что вы разрешили загрузку. (команда `modules setallow` ниже)

## Управление модулями

После запуска сервера, вы можете вводить команды в консоль:

- `modules list` - показывает загруженные модули.
- `modules allowlist` - показывает разрешенные модули.
- `modules setallow` `<название с .dll> <true | false>` - разрешает модуль или наоборот.
    - Пример: `modules setallow MyModule.dll true`

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
    private static bool IsInitialized;
    [ModuleInitialize]
    public static void Initialize()
    {
        if (IsInitialized) return;
        IsInitialized = true;

        // код инициализации модуля
    }
}
```

После того как вы создали модуль, его нужно скомпилировать: `dotnet build -c Release`.

В папке `bin/Release/net7.0` будет скомпилированный модуль. В нашем случае `MyModule.dll`

Переместите его в папку `extensions/modules` в корневой папке Amethyst.

Затем запустите сервер, и введите команду `/modules setallow MyModule.dll`, а затем перезагрузите сервер.

Вот и все, первый ваш модуль уже работает!

### Зависимости
Если вам понадобилась какая-либо сторонняя библиотека, то указав параметры `dependencies` в атрибутах модуля, API загрузит его.

Например, нам нужна библиотека расположенная по пути `deps/MyEpicLibrary.dll`:

Нужно указать путь к зависимости:

```cs
using Amethyst.Extensions.Modules;

namespace MyModule;

[AmethystModule("MyModule", "deps/MyEpicLibrary.dll")]
public static class MyModule
...
```

Можно подключать также несколько зависимостей:

```cs
using Amethyst.Extensions.Modules;

namespace MyModule;

[AmethystModule("MyModule", "deps/Library1.dll", "deps/Library2.dll", "deps/Library3.dll")]
public static class MyModule
...
```