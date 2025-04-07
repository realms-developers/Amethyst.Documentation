# Using MongoDB

Amethyst primarily uses MongoDB for data storage, providing convenient database operations.

## Data Models
Create models by inheriting from `DataModel`:
```cs
using Amethyst.Storages.Mongo;
using MongoDB.Bson.Serialization.Attributes;

[BsonIgnoreExtraElements]
public sealed class MyModel : DataModel
{
    public static MongoModels<MyModel> Models { get; } = MongoDatabase.Main.Get<MyModel>();

    public MyModel(string name) : base(name)
    {
        TestField = $"TestField: {name}";
    }

    public int TestField;
    public bool TestField2;
    public string TestField3;

    public override void Save() => Models.Save(this);
    public override void Remove() => Models.Remove(Name);
}
```

Key features:

- `Name` property acts as primary key
- Unique names enforced - no duplicate models
- Built-in `Save()` and `Remove()` methods

## MongoDatabase Class
Main database instance: `MongoDatabase.Main`

Connect to alternate databases:
```cs
// Connect to config-defined MongoDB with different database name
var alternateDb = new MongoDatabase(
    AmethystSession.StorageConfiguration.MongoConnection,
    "AlternateDB");

// Connect to custom MongoDB instance
var customDb = new MongoDatabase(
    "mongodb://alternateIP:27017/",
    "AnotherDB");
```

## MongoModels Class
Handles database collection operations:
```cs
MongoModels<MyModel> models = MongoDatabase.Main.Get<MyModel>();

// Create and save models
var model = new MyModel("UniqueName");
models.Save(model);

// Update model
model.TestField = 5;
models.Save(model);

// Remove models
models.Remove("UniqueName");
models.Remove(m => m.Name.StartsWith("M"));

// Find models
MyModel? foundModel = models.Find("UniqueName");
```