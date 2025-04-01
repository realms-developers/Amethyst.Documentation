# Utilities in PlayerUtilities

## BroadcastPacket
```cs
// Send packet to all players
public static void BroadcastPacket(byte[] data);

// Send packet to players matching a predicate
// Example: BroadcastPacket(data, plr => plr.Name.StartsWith("A"));
public static void BroadcastPacket(byte[] data, Predicate<NetPlayer> predicate);
```

## BroadcastText
```cs
// Send colored text to players matching a predicate
// Example: BroadcastText("Alert!", Color.Red, plr => plr.IsActive);
public static void BroadcastText(string text, Color color, Predicate<NetPlayer>? predicate = null);

// Send localized text to players matching a predicate
// Example: BroadcastText("amethyst.broadcast.example", Color.Orange);
public static void BroadcastText(string text, Color color, Predicate<NetPlayer>? predicate = null);
```