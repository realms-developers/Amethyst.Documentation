# MySQL

Amethyst поддерживает MySQL через класс `MySQLProvider` для SQL-операций.

## Управление подключением
```cs
// Использование строки подключения
var provider = new MySQLProvider("Server=localhost;Database=test;Uid=user;Pwd=pass;");

// Использование параметров
var provider = new MySQLProvider("localhost", "test", "user", "pass");

// Явное открытие подключения
provider.OpenConnection();

// Закрытие подключения
provider.CloseConnection();
```

## Основные операции
### Выполнение запроса (INSERT/UPDATE/DELETE)
```cs
int affectedRows = provider.ExecuteNonQuery(
    "INSERT INTO users (name, age) VALUES (@name, @age)",
    new Dictionary<string, object> {
        {"@name", "John"},
        {"@age", 25}
    });
```

### Получение скалярного значения
```cs
object? result = provider.ExecuteScalar(
    "SELECT COUNT(*) FROM users");
```

### Выполнение выборки
```cs
var data = provider.ExecuteQuery(
    "SELECT * FROM users WHERE age > @minAge",
    new Dictionary<string, object> {
        {"@minAge", 18}
    });
```