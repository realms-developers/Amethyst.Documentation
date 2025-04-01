# Players

The core player class is `NetPlayer`.

The main class responsible for player management is `Amethyst.Players.PlayerManager`.

The most essential property is Tracker.

`PlayerTracker` - a class (property `PlayerManager.Tracker`) that stores instances of `NetPlayer`.

Get a player instance via indexer:
```cs
int index = 1;
NetPlayer player = PlayerManager.Tracker[index];
```

# NetPlayer Properties
```cs
// Player's network ID
public int Index { get; }

// Player's nickname
public string Name => _playerName;

// Player's IP address
public string IP { get; set; }

// Player's UUID
public string UUID { get; set; }

// Command sender type. In this case - real player (SenderType.RealPlayer)
public SenderType Type => SenderType.RealPlayer;

// Reference to Terraria.Player
public Player TPlayer => Main.player[Index];

// Network interaction interface (e.g., packet sending)
public INetworkClient Socket => NetworkManager.Provider.GetClient(Index)!;

// SSC character wrapper
public ICharacterWrapper? Character { get; internal set; }

// Player management utilities
public LocalPlayerUtils Utils { get; }

// Indicates player is connected to server
public bool IsActive => Socket.IsConnected && !Socket.IsFrozen;

// Indicates player is connected and capable of server interactions:
// world interactions, command execution, etc.
public bool IsCapable => IsActive && UUID != "" && Name != "" && _wasSpawned;

// Indicates player has full permissions
// Enabled via /grantroot command in debug mode or plugins
public bool IsRootGranted { get; set; }

// Player's language. Default: set via launch arguments
// ru-RU, en-US, or plugin-added languages
public string Language { get; set; }
```

# Accessing Terraria.Player
`Player` - Terraria's player class. Do not confuse with core's `NetPlayer`.

```cs
using Terraria;

NetPlayer player;

Player terrariaPlayer = player.TPlayer;
terrariaPlayer.statLife = 100;
```

# Iteration
`PlayerTracker` implements `IEnumerable<NetPlayer>`, allowing `foreach` usage:
```cs
foreach (NetPlayer player in PlayerManager.Tracker)
{
    Console.WriteLine($"Found player {player.Name} in Tracker");
}
```

Additional filtered collections:
```cs
foreach (NetPlayer player in PlayerManager.Tracker.Capable)
    Console.WriteLine($"Found capable player: {player.Name}");

foreach (NetPlayer player in PlayerManager.Tracker.Alive)
    Console.WriteLine($"Found alive player: {player.Name}");

foreach (NetPlayer player in PlayerManager.Tracker.Dead)
    Console.WriteLine($"Found dead player: {player.Name}");
```

# Player Search
`PlayerTracker` has `NetPlayer? FindPlayer(string nameOrIndex)` method:
```cs
NetPlayer? player = PlayerManager.Tracker.FindPlayer("vxlhat") ?? 
                    PlayerManager.Tracker.FindPlayer("VXL") ?? 
                    PlayerManager.Tracker.FindPlayer("vxlh") ?? 
                    PlayerManager.Tracker.FindPlayer("1"); // Player ID. Better use PlayerManager.Tracker[1];

if (player != null)
    Console.WriteLine("vxlhat found");
```