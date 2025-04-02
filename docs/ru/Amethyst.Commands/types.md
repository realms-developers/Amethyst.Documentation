## Типы команд

Определены в перечислении `CommandType`:
```cs
public enum CommandType
{
    /// <summary>
    /// Доступна всем с соответствующими правами.
    /// </summary>
    Shared,

    /// <summary>
    /// Доступна через консоль или RCON.
    /// </summary>
    Console,

    /// <summary>
    /// Доступна в режиме разработчика.
    /// </summary>
    Debug
}
```

### Использование

| Тип        | Типичные сценарии                  |
|------------|-------------------------------------|
| `Shared`   | Команды для игроков                 |
| `Console`  | Администрирование/фоновые задачи    |
| `Debug`    | Инструменты разработки/тестирования |

Пример отладочной команды:
```cs
[ServerCommand(CommandType.Debug, "devtool", "Инструмент разработчика", null)]
public static void DevToolCommand(CommandInvokeContext ctx) { }
```