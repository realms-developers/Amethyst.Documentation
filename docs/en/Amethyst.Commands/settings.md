# Settings

Defined in `CommandSettings` enum:
```cs
[Flags]
public enum CommandSettings
{
    /// <summary>
    /// Executed only in-game.
    /// </summary>
    IngameOnly = 1,

    /// <summary>
    /// Suppress execution logging.
    /// </summary>
    HideLog = 2,

    /// <summary>
    /// Exclude from help listings.
    /// </summary>
    Hidden = 4
}
```

Control command visibility and logging:

```cs
[CommandsSettings(CommandSettings.Hidden | CommandSettings.NoLog)]
public static void SecretCommand(CommandInvokeContext ctx)
{
    // Hidden from help systems and execution not logged
}
```