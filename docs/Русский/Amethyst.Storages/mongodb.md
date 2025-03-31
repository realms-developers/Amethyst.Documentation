# Использование MongoDB

Основная база данных, которая используется в Amethyst - MongoDB. Amethyst предоставляет удобное и быстрое использование этой базы данных.

Чтобы использовать эту базу данных, необходимо создать класс, который наследует `DataModel`.

Модель (`DataModel`) - это структура данных.

К примеру, базовая модель выглядит так:

```cs
using Amethyst.Storages.Mongo;
using MongoDB.Bson.Serialization.Attributes;

[BsonIgnoreExtraElements] // мне слишком лень разъяснять за это но оно вас спасет
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

    public override void Save()
    {
        Models.Save(this);
    }

    public override void Remove()
    {
        Models.Remove(Name);
    }
}
```

`DataModel` имеет primary-key `Name` (название, по которому модель сохраняется в БД).

Amethyst не позволяет иметь несколько моделей под одним названием.

`Name` устанавливается в конструкторе:

```cs
public CharacterModel(string name) : base(name)
```

Базовые методы `DataModel`:

- Метод `Save` - сохраняет модель в базу данных
- Метод `Remove` - удаляет модель из базы данных.

Базовые свойства `DataModel`:

- Свойство `Name` - primary-key.

# Класс MongoDatabase

`MongoDatabase` - класс, обертка над определенной базы данных в MongoDB.

Экземпляр основной базы данных (которая установлена в конфиге) находится в `MongoDatabase.Main`.

Чтобы подключиться к другой базе данных, можно использовать конструктор:

```cs
MongoDatabase mainDb = MongoDatabase.Main;

// подключается к MongoDB по адресу который указан в конфиге, но использует другую базу данных в MongoDB.
MonogDatabase alternateDb = new MongoDatabase(
        AmethystSession.StorageConfiguration.MongoConnection,
        "AlternateDB");

// подключается к базе данных по alternateIP : 27017 
MonogDatabase alternateDb = new MongoDatabase(
        "mongodb://alternateIP:27017/",
        "AnotherAlternateDB");
```

# Класс MongoModels

`MongoModels<T>` - класс, обертка над MongoDB. Именно через этот класс происходят все манипуляции с определенной коллекцией в БД.

Например для SSC-персонажей (`CharacterModel`) существует `MongoModels<CharacterModel>`, экземпляр которого находится в `PlayerManager.Characters`.

Чтобы получить экземпляр для вашей модели, используйте `Get<T>` в `MongoDatabase`:

```cs
MongoDatabase mainDb = MongoDatabase.Main;

MongoModels<MyModel> models = mainDb.Get<MyModel>();
```

Как сказано выше, через класс `MongoModels<T>` проходят все манипуляции с коллекцией в базе данных:

```cs
MongoDatabase mainDb = MongoDatabase.Main;
MongoModels<MyModel> models = mainDb.Get<MyModel>();

MyModel model = new MyModel("MyModelName");
MyModel model2 = new MyModel("ModelName2");

// добавляет или перезаписывает модель
// т.е. если модель с таким названием уже есть в БД, Amethyst перезапишет его.
models.Save(model);

model.TestField = 5;
models.Save(model);

model.TestField = 10;
models.Save(model);

// удаляет модель
bool wasRemoved = models.Remove("MyModelName");
bool wasRemoved = models.Remove(model.Name);
bool wasRemoved = models.Remove(predicate => predicate.Name.StartsWith("M"));

// ищет модель
// если модели нет в БД, возвращает null
// если есть - возвращает модель из БД.
MyModel? model3 = models.Find("MyModelName");
```

