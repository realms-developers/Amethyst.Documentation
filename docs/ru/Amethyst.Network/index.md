# Взаимодействие с сетью

В Terraria пакеты делятся на два типа: обычный пакет (далее raw), модули (далее net-module).

Amethyst предоставляет возможность изменять поведение пакетов: входящие/исходящие raw-пакеты, входящие net-module.

# Входящие raw-пакеты

Для обработки используется `NetworkManager.Binding.AddInPacket(...)`, пример использования:

```cs
NetworkManager.Binding.AddInPacket(PacketTypes.ItemOwner, OnItemOwner);

// или

NetworkManager.Binding.AddInPacket(22, OnItemOwner);

void OnItemOwner(in IncomingPacket packet, PacketHandleResult result)
{   
    int start = packet.Start; // позиция начала пакета
    int length = packet.Length; // длина пакета
    byte senderIndex = packet.Sender; // индекс отправителя
    NetPlayer player = packet.Player; // экземпляр NetPlayer отправителя (тоже самое что и packet.Sender)

    BinaryReader reader = packet.GetReader();

    // структура пакета 'ItemOwner':
    short itemIndex = reader.ReadInt16(); // индекс предмета
	byte targetPlayerIndex = reader.ReadByte(); // новый владелец предмета

    // ... какой то очень важный код 

    // игнорирование пакета
    // причина игнорирования пакета например: использование читов, неверные значения
    result.Ignore("<ignore reason>");
}
```

# Входящие net-module-пакеты

Для обработки используется `NetworkManager.Binding.AddInModule(...)`, пример использования:

```cs
NetworkManager.Binding.AddInModule(ModuleTypes.MapPing, OnMapPing);

// или

NetworkManager.Binding.AddInModule(2, OnMapPing);

void OnMapPing(in IncomingModule packet, PacketHandleResult result)
{
    BinaryReader reader = packet.GetReader();

    // структура модуля `MapPing`:
    // так же можно использовать так:
    // Vector2 position = reader.ReadVector2();
    float x = reader.ReadSingle();
    float y = reader.ReadSingle();

    // ... какой то очень важный код 

    // игнорирование пакета
    // причина игнорирования пакета например: использование читов, неверные значения
    result.Ignore("<ignore reason>");
}
```

# Исходящие raw-пакеты

Для обработки используется `NetworkManager.Binding.AddOutPacket(...)`, пример использования:

Рекомендуем декомпилировать исходный код класса `Terraria.NetMessage` из `deps/TerrariaAPI.dll`, для полного понимания картины.

```cs
NetworkManager.Binding.AddOutPacket(PacketTypes.ItemOwner, OnItemOwner);

// или

NetworkManager.Binding.AddOutPacket(22, OnItemOwner);

void OnItemOwner(in OutcomingModule packet, PacketHandleResult result)
{
    // не поддерживает BinaryReader и прочее, так как оно не читает пакет.

    int remoteClient = packet.RemoteClient;
    int ignoreClient = packet.IgnoreClient;
    NetworkText? text = packet.Text;
    int num1 = packet.Number1;
    float num2 = packet.Number2;
    float num3 = packet.Number3;
    float num4 = packet.Number4;
    int num5 = packet.Number5;
    int num6 = packet.Number6;
    int num7 = packet.Number7;

    // игнорирование пакета
    // причина игнорирования пакета например: ненужная отправка данных
    result.Ignore("<ignore reason>");
}
```