# Localization

Amethyst has built-in localization. To verify, check the `localization` folder in the core directory.

Initially, you will see these files:
```
├── en-US
│   ├── en-US.amethyst.json
│   ├── en-US.commands.desc.json
│   ├── en-US.commands.json
│   ├── en-US.commands.text.json
│   ├── en-US.network.json
│   └── en-US.packet.json
└── ru-RU
    ├── ru-RU.amethyst.json
    ├── ru-RU.commands.desc.json
    ├── ru-RU.commands.json
    ├── ru-RU.commands.text.json
    ├── ru-RU.network.json
    └── ru-RU.packet.json
```

To extend localization (e.g., when installing a plugin), simply add localization files to `en-US` and `ru-RU`.

## Adding a Custom Language
To add a new language, create a new directory and add localization files.

For example, to add Latin as a joke, create a `la-NA` folder alongside `ru-RU` and `en-US`.

Next, enable the language:

```cs
[ServerCommand(CommandType.Shared, "lang la", "set lingua latina.", null)]
public static void Latin(CommandInvokeContext ctx)
{
    ctx.Sender.Language = "la-NA";
    ctx.Sender.ReplySuccess("Num quis hoc legit?");
}
```

If you want to enforce Latin for all players, start the server with the `-language la-NA` argument.

## Adding Localization Files

A typical localization file uses `key:value` pairs:
```json
{
    "myKey": "My text"
}
```

To use localization, call `Localization.Get`:
```cs
{
    string text = Localization.Get("myKey", "la-NA");
    Console.WriteLine($"My text is: {text}");
}
```

To display localized text to a player in their language:
```cs
{
    NetPlayer player; // Assume we have a player object.

    string text = Localization.Get("myKey", player.Language);
    player.ReplyInfo(text);
}
```