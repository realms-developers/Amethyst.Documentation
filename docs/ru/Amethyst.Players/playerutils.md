# Утилиты в PlayerUtilities

## BroadcastPacket

```cs
// отправляет пакет всем игрокам
public static void BroadcastPacket(byte[] data);

// отправляет пакет всем игрокам, которые соответствуют Predicate<NetPlayer>
// например: BroadcastPacket(data, (plr) => plr.Name.StartsWith("Text"));
public static void BroadcastPacket(byte[] data, Predicate<NetPlayer> predicate)
```

## BroadcastText

```cs
// отправляет сообщение всем игрокам, которые соответствуют Predicate<NetPlayer>
// например: BroadcastText("My text", new Color(255, 0, 0), (plr) => plr.IsActive);
public static void BroadcastText(string text, Color color, Predicate<NetPlayer>? predicate = null);

// отправляет локализованное сообщение всем игрокам, которые соответствуют Predicate<NetPlayer>
// например: BroadcastText("amethyst.broadcast.example", new Color(255, 0, 0), (plr) => plr.IsActive);
public static void BroadcastText(string text, Color color, Predicate<NetPlayer>? predicate = null);
```