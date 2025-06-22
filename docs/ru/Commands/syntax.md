# Синтаксис

## Множественные шаблоны

Поддержка альтернативных вариантов через `[CommandsSyntax]`:
```cs
[CommandsSyntax("[score]", "[-s(ilent)]")]
public static void ScoreCommand(
    CommandInvokeContext ctx, 
    int score = 0, 
    string args = ""
)
{
    // Обработка флага -s в args
    if (args.Contains("-s")) { /* Тихий режим */ }
}
```

## Нотация синтаксиса
| Шаблон       | Описание                          |
|--------------|-----------------------------------|
| `<required>` | Обязательный параметр             |
| `[optional]` | Опциональный параметр             |
| `-f(lag)`    | Флаг-переключатель (кор./полн.)   |

## Расширенные возможности

### Пользовательские парсеры
```cs
public static void AdvancedCommand(
    CommandInvokeContext ctx,
    [ArgumentParser(typeof(CommandParsers), nameof(ParseCustom))]
    CustomType param
)
{
    // Использование кастомного значения
}

public static ParseResult ParseCustom(ICommandSender sender, string input)
{
    // Логика парсинга
    return ParseResult.Success(new CustomType(input));
}
```

### Обработка временных интервалов
```cs
public static void TempBan(CommandInvokeContext ctx, string duration)
{
    int seconds = TextUtility.ParseToSeconds("1h30m"); // = 5400
    // Реализация бана
}
```

#### Поддерживаемые форматы:
- `1d2h30m` → 1 день + 2 часа + 30 минут
- `3600s` → 3600 секунд
- `2h` → 7200 секунд