# Network Interaction

In Terraria, packets are divided into two types: raw packets and net-module packets.

Amethyst provides capabilities to modify packet behavior: incoming/outgoing raw packets and incoming net-modules.

# Incoming Raw Packets

Use `NetworkManager.Binding.AddInPacket(...)` for handling. Example:

```cs
NetworkManager.Binding.AddInPacket(PacketTypes.ItemOwner, OnItemOwner);

// or

NetworkManager.Binding.AddInPacket(22, OnItemOwner);

void OnItemOwner(in IncomingPacket packet, PacketHandleResult result)
{   
    int start = packet.Start; // packet start position
    int length = packet.Length; // packet length
    byte senderIndex = packet.Sender; // sender's index
    NetPlayer player = packet.Player; // NetPlayer instance of the sender (same as packet.Sender)

    BinaryReader reader = packet.GetReader();

    // Structure of 'ItemOwner' packet:
    short itemIndex = reader.ReadInt16(); // item index
    byte targetPlayerIndex = reader.ReadByte(); // new item owner

    // ... some very important code

    // Ignore the packet
    // Example reasons: cheating detected, invalid values
    result.Ignore("<ignore reason>");
}
```

# Incoming Net-Module Packets

Use `NetworkManager.Binding.AddInModule(...)` for handling. Example:

```cs
NetworkManager.Binding.AddInModule(ModuleTypes.MapPing, OnMapPing);

// or

NetworkManager.Binding.AddInModule(2, OnMapPing);

void OnMapPing(in IncomingModule packet, PacketHandleResult result)
{
    BinaryReader reader = packet.GetReader();

    // Structure of 'MapPing' module:
    // Alternative: Vector2 position = reader.ReadVector2();
    float x = reader.ReadSingle();
    float y = reader.ReadSingle();

    // ... some very important code

    // Ignore the packet
    // Example reasons: cheating detected, invalid values
    result.Ignore("<ignore reason>");
}
```

# Outgoing Raw Packets

Use `NetworkManager.Binding.AddOutPacket(...)` for handling. Example:

We recommend decompiling the `Terraria.NetMessage` class from `deps/TerrariaAPI.dll` for full context.

```cs
NetworkManager.Binding.AddOutPacket(PacketTypes.ItemOwner, OnItemOwner);

// or

NetworkManager.Binding.AddOutPacket(22, OnItemOwner);

void OnItemOwner(in OutcomingModule packet, PacketHandleResult result)
{
    // BinaryReader not supported here as the packet isn't read

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

    // Ignore the packet
    // Example reason: unnecessary data transmission
    result.Ignore("<ignore reason>");
}
```