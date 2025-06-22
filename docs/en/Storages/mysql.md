# MySQL

Amethyst supports MySQL for SQL-based data storage operations through the `MySQLProvider` class.

## Connection Management
```cs
// Using connection string
var provider = new MySQLProvider("Server=localhost;Database=test;Uid=user;Pwd=pass;");

// Using individual parameters
var provider = new MySQLProvider("localhost", "test", "user", "pass");

// Open connection explicitly
provider.OpenConnection();

// Close connection when done
provider.CloseConnection();
```

## Basic Operations
### Execute Non-Query (INSERT/UPDATE/DELETE)
```cs
int affectedRows = provider.ExecuteNonQuery(
    "INSERT INTO users (name, age) VALUES (@name, @age)",
    new Dictionary<string, object> {
        {"@name", "John"},
        {"@age", 25}
    });
```

### Execute Scalar
```cs
object? result = provider.ExecuteScalar(
    "SELECT COUNT(*) FROM users");
```

### Execute Query
```cs
var data = provider.ExecuteQuery(
    "SELECT * FROM users WHERE age > @minAge",
    new Dictionary<string, object> {
        {"@minAge", 18}
    });
```