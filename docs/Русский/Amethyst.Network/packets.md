# Создание и отправка пакетов

`PacketWriter` - класс-инструмент, который предназначен для удобного создания пакетов.

Например:

```cs
// создание пакета
byte[] packet = new PacketWriter()
    .SetType(5) // PacketTypes.PlayerSlot (пакет для обновления инвентаря)

    .PackByte(1)        // индекс игрока
    .PackInt16(10)      // 11 слот (т.к. 1 слот в привычном понимании равен 0 слоту в коде)
    .PackInt16(9999)    // кол-во предмета
    .PackByte(5)        // ID зачарования
    .PackInt16(757)     // ID предмета

    .BuildPacket(); // конвертирует пакет в буфер (byte[])

NetPlayer player;

// отправляет пакет локально одному определенному игроку
player.Socket.SendPacket(packet);

// отправляет пакет всем игрокам, кроме `player`
PlayerUtilities.BroadcastPacket(packet, predicate => predicate.Index != player.Index);
```