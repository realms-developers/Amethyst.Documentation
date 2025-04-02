# Настройки

Определены в перечислении `CommandSettings`:
```cs
[Flags]
public enum CommandSettings
{
    /// <summary>
    /// Выполняется только внутри игры.
    /// </summary>
    IngameOnly = 1,

    /// <summary>
    /// Отключает логирование выполнения.
    /// </summary>
    HideLog = 2,

    /// <summary>
    /// Исключает из списков помощи.
    /// </summary>
    Hidden = 4
}
```

Управление видимостью и логированием команд:
```cs
[CommandsSettings(CommandSettings.Hidden | CommandSettings.HideLog)]
public static void SecretCommand(CommandInvokeContext ctx)
{
    // Скрыта из справки, выполнение не логируется
}
```