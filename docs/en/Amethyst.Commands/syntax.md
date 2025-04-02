# Syntax

## Multiple Syntax Patterns

Support alternative command usages with `[CommandsSyntax]`:
```cs
[CommandsSyntax("[score]", "[-s(ilent)]")]
public static void ScoreCommand(
    CommandInvokeContext ctx, 
    int score = 0, 
    string args = ""
)
{
    // Handle -s flag in args
    if (args.Contains("-s")) { /* Silent mode */ }
}
```

## Syntax Notation
| Pattern       | Description                          |
|---------------|--------------------------------------|
| `<required>`  | Mandatory parameter                 |
| `[optional]`  | Optional parameter                  |
| `-f(lag)`     | Switch flag (short/long form)       |

## Advanced Features

### Custom Argument Parsers
```cs
public static void AdvancedCommand(
    CommandInvokeContext ctx,
    [ArgumentParser(typeof(CommandParsers), nameof(ParseCustom))]
    CustomType param
)
{
    // Use custom parsed value
}

public static ParseResult ParseCustom(ICommandSender sender, string input)
{
    // Custom parsing logic
    return ParseResult.Success(new CustomType(input));
}
```

### Time Duration Handling
```cs
public static void TempBan(CommandInvokeContext ctx, string duration)
{
    int seconds = TextUtility.ParseToSeconds("1h30m"); // = 5400
    // Ban implementation
}
```

#### Supported Time Formats:
- `1d2h30m` → 1 day + 2 hours + 30 minutes
- `3600s` → 3600 seconds
- `2h` → 7200 seconds