# JSON-конфигурация

Amethyst использует JSON в качестве основной конфигурации и предоставляет удобное использование конфигураций в коде.

Для конфигурации требуется `struct`-тип, который имеет в себе необходимые данные:

```cs
public struct MyConfigData
{
    public int TestValue;
    public string? TestText;
    public string DefaultTestText = "Hello World!";
    public List<string> TestValues;
}
```

Класс `Configuration<T>` - инструмент, который управляет вашей конфигурацией: загружает, сохраняет, изменяет.

Пример использования:

```cs
Configuration<MyConfigData> config = new Configuration<MyConfigData>(typeof(MyConfigData).FullName!, new());

// загружает конфиг
config.Load();

// инициализация стандартных значений в конфиге
config.Modify(InitializeConfig, true);

void InitializeConfig(ref MyConfigData data)
{
    if (data.TestText == null)
    {
        data.TestText = "default value of TestText";
    }
    
    // Нет необходимости инициализировать DefaultTestText
    
    if (data.TestValues == null)
    {
        data.TestValues = new List<string>();
    }
}
```

Чтобы получить экземпляр `MyConfigData`, нужно использовать свойство `Data`:

```cs
MyConfigData data = config.Data;

if (data.TestText == "default value of TestText")
{
    Console.WriteLine("Значение по умолчанию!");
}
```