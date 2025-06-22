## Command Types

Defined in `CommandType` enum:
```cs
public enum CommandType
{
    /// <summary>
    /// Can used be by everyone who have permission.
    /// </summary>
    Shared,

    /// <summary>
    /// Can be used by console or RCON.
    /// </summary>
    Console,

    /// <summary>
    /// Can be used in developer server mode.
    /// </summary>
    Debug
}
```

### Usage

| Type      | Typical Use Case                     |
|-----------|--------------------------------------|
| `Shared`  | Player-facing commands               |
| `Console` | Administrative/background tasks      |
| `Debug`   | Development/testing utilities        |

Example Debug Command:
```cs
[ServerCommand(CommandType.Debug, "devtool", "Developer tool", null)]
public static void DevToolCommand(CommandInvokeContext ctx) { }
```