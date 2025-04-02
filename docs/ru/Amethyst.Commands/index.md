# Команды

## Справочник классов  

### Основные классы
| Класс/Интерфейс               | Описание                                      |
|-------------------------------|-----------------------------------------------|
| `CommandData`                 | Контейнер метаданных для зарегистрированных команд |
| `CommandInvokeContext`        | Контекст выполнения с аргументами и отправителем |
| `ICommandSender`              | Интерфейс для инициаторов команд              |
| `TextPage`                    | Контейнер для постраничного контента          |

### Атрибуты
| Атрибут                     | Назначение                                   |
|-----------------------------|----------------------------------------------|
| `ServerCommandAttribute`    | Определяет метаданные регистрации команд     |
| `CommandsSyntaxAttribute`   | Задает шаблоны синтаксиса команд             |
| `CommandsSettingsAttribute` | Настраивает флаги поведения команд           |

## Структура методов

Команды реализуются как статические методы с атрибутами:

```cs
[ServerCommand(
    CommandType.Shared, 
    "example", 
    "Пример описания команды", 
    "example.permission"
)]
[CommandsSyntax("<обязательный_параметр>", "[опциональный_параметр]")]
public static void ExampleCommand(
    CommandInvokeContext ctx, 
    string required_param, 
    int optional_param = 0
)
{
    // Логика команды
    ctx.Sender.ReplySuccess($"Выполнено с {required_param} и {optional_param}");
}
```

### Основные компоненты:
1. **`ServerCommandAttribute`**  
    - `CommandType`: Уровень доступности команды
    - `Name`: Имя для вызова команды
    - `Description`: Описание для справки
    - `Permission`: Требуемый узел разрешений (null — без разрешений)

2. **`CommandInvokeContext`**  
    Содержит контекст выполнения:
    - `Sender`: Контекст инициатора (реализует `ICommandSender`)
    - `ArgumentsText`: Необработанная строка аргументов
    - `Arguments`: Токенизированный список аргументов

3. **`ICommandSender`**  
    Интерфейс для источников команд с методами:
    - Проверка разрешений (`HasPermission()`)
    - Ответы (`ReplySuccess()`, `ReplyError()` и т.д.)
    - Поддержка локализации через свойство `Language`

## Система разрешений

Рекомендации:
- Используйте иерархию через точку: `модуль.функция.действие`
- Проверяйте разрешения на ранних этапах выполнения

### Статические разрешения

```cs
[ServerCommand(CommandType.Shared, "ban", "Забанить игрока", "moderation.ban")]
```

### Динамические проверки

```cs
if (!ctx.Sender.HasPermission("special.access"))
{
    ctx.Sender.ReplyError("Доступ запрещён!");
    return;
}
```

## Обработка ответов

### Типы ответов
```cs
ctx.Sender.ReplySuccess("Операция завершена");    // Зеленый текст
ctx.Sender.ReplyInfo("Статус: онлайн");          // Желтый текст
ctx.Sender.ReplyWarning("Мало ресурсов");        // Оранжевый текст
ctx.Sender.ReplyError("Некорректный ввод");      // Красный текст
```

### Форматированные сообщения
```cs
ctx.Sender.ReplySuccess("Игрок {0} набрал {1} очков", playerName, score);
```

## Постраничная система

### Создание контента

```cs
var items = Enumerable.Range(1, 100).Select(i => $"Элемент {i}");
var pages = PagesCollection.CreateFromList(
    items, 
    maxLineLength: 80,   // Символов на строку
    linesPerPage: 10     // Строк на страницу
);
```

### Отправка страниц

```cs
ctx.Sender.ReplyPage(
    pages,
    header: "Список элементов - Страница {0} из {1}",
    footer: "Используйте /example <страница> для навигации",
    footerArgs: null,
    showPageName: true,
    page: 2
);
```

## Поддержка локализации

- Использует свойство `ICommandSender.Language`
- Интегрируется с системой переводов

### Локализованные команды
```cs
[ServerCommand(CommandType.Shared, "help", "commands.help.desc", null)]
```

### Локализованные ответы
```cs
ctx.Sender.ReplySuccess("commands.help.success");
```