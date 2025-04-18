# JSON Configuration

Amethyst uses JSON as its primary configuration format and provides convenient configuration handling in code.

A configuration requires a `struct` type containing the necessary data:
```cs
public struct MyConfigData
{
    public int TestValue;
    public string? TestText;
    public string DefaultTestText = "Hello World!";
    public List<string> TestValues;
}
```

The `Configuration<T>` class manages your configuration - loading, saving, and modifying it.

Example usage:
```cs
Configuration<MyConfigData> config = new Configuration<MyConfigData>(typeof(MyConfigData).FullName!, new());

// Load configuration
config.Load();

// Initialize values
config.Modify(InitializeConfig, true);

void InitializeConfig(ref MyConfigData data)
{
    if (data.TestText == null)
    {
        data.TestText = "default value of TestText";
    }
    
    // No need to initialize DefaultTestText
    
    if (data.TestValues == null)
    {
        data.TestValues = new List<string>();
    }
}
```

Access configuration data through the `Data` property:
```cs
MyConfigData data = config.Data;

if (data.TestText == "default value of TestText")
{
    Console.WriteLine("Value is default!");
}
```