# Локализация

Amethyst имеет встроенную локализацию, если не верите - смотрите в папку `localization` в корневой папке ядра.

По началу, вы увидите только эти файлы:

```
├── en-US
│   ├── en-US.amethyst.json
│   ├── en-US.commands.desc.json
│   ├── en-US.commands.json
│   ├── en-US.commands.text.json
│   ├── en-US.network.json
│   └── en-US.packet.json
└── ru-RU
    ├── ru-RU.amethyst.json
    ├── ru-RU.commands.desc.json
    ├── ru-RU.commands.json
    ├── ru-RU.commands.text.json
    ├── ru-RU.network.json
    └── ru-RU.packet.json
```

Чтобы дополнить локализацию (например при установке плагина), просто перекиньте файлы локализации в en-US и ru-RU.

## Свой язык
А для того, чтобы добавить свой язык, нужно создать отдельную папку, и внести туда файлы локализации.

Допустим, мне стало плохо и мне причудилось что Amethyst обязательно должен иметь латинский язык: я создаю папку `la-NA` наряду с `ru-RU` и `en-US`

С этим разобрались, но теперь можно и добавить включение этого языка.

```cs
[ServerCommand(CommandType.Shared, "lang la", "set lingua latina.", null)]
public static void LangRU(CommandInvokeContext ctx)
{
    ctx.Sender.Language = "la-NA";
    ctx.Sender.ReplySuccess("Num quis hoc legit?");
}
```

Но, если вы кровожадный диктатор на вашем сервере - можно для всех установить латинский язык, просто запустив сервер с аргументом `-language la-NA`

## Добавление файлов локализации

Среднестатистический файл локализации состоит из выражений `ключ:значение`:

```json
{
    "myKey": "My text"
}
```

Чтобы использовать локализацию, используйте `Localization.Get`:

```cs
{
    string text = Localization.Get("myKey", "la-NA");
    Console.WriteLine($"My text is: {text}");
}
```

Чтобы вывести игроку локализованный текст на выбранном им языке:

```cs
{
    NetPlayer player; // допустим, у нас есть какой то игрок.

    string text = Localization.Get("myKey", player.Language);
    player.ReplyInfo(text);
}
```