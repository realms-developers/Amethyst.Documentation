# Commands

## Class Reference 

### Core Classes
| Class/Interface               | Description                                      |
|-------------------------------|--------------------------------------------------|
| `CommandData`                 | Metadata container for registered commands       |
| `CommandInvokeContext`        | Execution context with arguments and sender      |
| `ICommandSender`              | Interface for command invokers                   |
| `TextPage`                    | Paginated content container                      |

### Attributes
| Attribute                    | Purpose                                        |
|------------------------------|------------------------------------------------|
| `ServerCommandAttribute`     | Defines command registration metadata          |
| `CommandsSyntaxAttribute`    | Specifies command syntax patterns              |
| `CommandsSettingsAttribute`  | Configures command behavior flags              |

## Method Structure

Commands are implemented as static methods decorated with attributes:

```cs
[ServerCommand(
    CommandType.Shared, 
    "example", 
    "Example command description", 
    "example.permission"
)]
[CommandsSyntax("<required_param>", "[optional_param]")]
public static void ExampleCommand(
    CommandInvokeContext ctx, 
    string required_param, 
    int optional_param = 0
)
{
    // Command logic
    ctx.Sender.ReplySuccess($"Executed with {required_param} and {optional_param}");
}
```

### Core Components:
1. **`ServerCommandAttribute`**  
    - `CommandType`: Command accessibility level
    - `Name`: Command invocation name
    - `Description`: Help text description
    - `Permission`: Required permission node (null for no permission)

2. **`CommandInvokeContext`**  
    Contains execution context:
    - `Sender`: Invoker's context (implements `ICommandSender`)
    - `ArgumentsText`: Raw unparsed arguments string
    - `Arguments`: Tokenized argument list

3. **`ICommandSender`**  
    Interface for command originators with:
    - Permission checks (`HasPermission()`)
    - Response methods (`ReplySuccess()`, `ReplyError()`, etc.)
    - Localization support via `Language` property

## Permission System

Prefer to:

- Use dot-separated hierarchy: `module.feature.action`
- Validate permissions early in command execution

### Static Permissions

```cs
[ServerCommand(CommandType.Shared, "ban", "Ban player", "moderation.ban")]
```

### Dynamic Checks

```cs
if (!ctx.Sender.HasPermission("special.access"))
{
    ctx.Sender.ReplyError("Access denied!");
    return;
}
```

## Response Handling

### Response Types
```cs
ctx.Sender.ReplySuccess("Operation completed");  // Green text
ctx.Sender.ReplyInfo("Status: Online");          // Yellow text
ctx.Sender.ReplyWarning("Low resources");        // Orange text
ctx.Sender.ReplyError("Invalid input");          // Red text
```

### Formatted Messages
```cs
ctx.Sender.ReplySuccess("Player {0} scored {1} points", playerName, score);
```

## Pagination System

### Creating Paginated Content

```cs
var items = Enumerable.Range(1, 100).Select(i => $"Item {i}");
var pages = PagesCollection.CreateFromList(
    items, 
    maxLineLength: 80,  // Characters per line
    linesPerPage: 10    // Lines per page
);
```

### Sending Pages

```cs
ctx.Sender.ReplyPage(
    pages,
    header: "Item List - Page {0} of {1}",
    footer: "Use /example <page> to navigate",
    footerArgs: null,
    showPageName: true,
    page: 2
);
```

## Localization Support

- Uses `ICommandSender.Language` property
- Integrates with translation files/system

### Localized Commands
```cs
[ServerCommand(CommandType.Shared, "help", "commands.help.desc", null)]
```

### Localized Responses
```cs
ctx.Sender.ReplySuccess("commands.help.success");
```