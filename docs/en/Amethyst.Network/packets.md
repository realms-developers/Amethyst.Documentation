# Creating and Sending Packets

`PacketWriter` - a utility class designed for convenient packet creation.

Example:

```cs
// Create a packet
byte[] packet = new PacketWriter()
    .SetType(5) // PacketTypes.PlayerSlot (inventory update packet)

    .PackByte(1)        // player index
    .PackInt16(10)      // slot 11 (slot numbering starts at 0 in code)
    .PackInt16(9999)    // item stack size
    .PackByte(5)        // prefix ID
    .PackInt16(757)     // item ID

    .BuildPacket(); // converts packet to byte buffer

NetPlayer player;

// Send packet locally to a specific player
player.Socket.SendPacket(packet);

// Broadcast packet to all players except `player`
PlayerUtilities.BroadcastPacket(packet, predicate => predicate.Index != player.Index);
```