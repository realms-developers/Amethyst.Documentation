# SQLite

Amethyst предоставляет поддержку SQLite через класс `SQLiteProvider`.

## Управление подключением
```cs
// Использование прямого пути
var provider = new SQLiteProvider("data.db");

// Использование профиля сервера
var provider = new SQLiteProvider(serverProfile);

// Автоматическое открытие подключения
provider.OpenConnection(); // Опционально - первый запрос откроет подключение
```

## Основные операции
### Создание таблицы
```cs
provider.ExecuteNonQuery(
    "CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY, name TEXT, age INTEGER)");
```

### Пакетная вставка
```cs
provider.ExecuteNonQuery(
    "INSERT INTO users (name, age) VALUES ('Alice', 30), ('Bob', 28)");
```

### Параметризованный запрос
```cs
var users = provider.ExecuteQuery(
    "SELECT * FROM users WHERE age BETWEEN @min AND @max",
    new Dictionary<string, object> {
        {"@min", 25},
        {"@max", 35}
    });
```