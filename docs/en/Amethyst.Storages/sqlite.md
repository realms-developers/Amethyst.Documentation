# SQLite

Amethyst provides SQLite support through the `SQLiteProvider` class for lightweight embedded database operations.

## Connection Management
```cs
// Using direct path
var provider = new SQLiteProvider("data.db");

// Using server profile
var provider = new SQLiteProvider(serverProfile);

// Automatic connection opening
provider.OpenConnection(); // Optional - first query will auto-open
```

## Basic Operations
### Create Table
```cs
provider.ExecuteNonQuery(
    "CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY, name TEXT, age INTEGER)");
```

### Batch Insert
```cs
provider.ExecuteNonQuery(
    "INSERT INTO users (name, age) VALUES ('Alice', 30), ('Bob', 28)");
```

### Parameterized Query
```cs
var users = provider.ExecuteQuery(
    "SELECT * FROM users WHERE age BETWEEN @min AND @max",
    new Dictionary<string, object> {
        {"@min", 25},
        {"@max", 35}
    });
```