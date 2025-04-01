# SSC

SSC (Server-Side Characters) - server mode where player characters are stored server-side.

Check if SSC is enabled via `bool PlayerManager.IsSSCEnabled`.

Player's character is stored in `ICharacterWrapper? NetPlayer.Character`.

## ICharacterWrapper Key Properties
```cs
// Character database model
public CharacterModel Model { get; }

// Indicates SSC is enabled and player can receive SSC packets
public bool SupportsChange { get; }
    
// Locks character modifications from player
// Only plugins/modules/core can modify characters
public bool IsReadonly { get; set; }
```

# Indexers
Two indexers in `ICharacterWrapper`:

Get inventory slot:
```cs
NetPlayer player;
ICharacterWrapper wrapper = player.Character!;

NetItem item = wrapper[5]; // Gets item in slot 6 (index 0 = first slot)
```

Get color values:
```cs
NetPlayer player;
ICharacterWrapper wrapper = player.Character!;

NetColor hairCol = wrapper[PlayerColorType.HairColor];
NetColor skinCol = wrapper[PlayerColorType.SkinColor];
```

# Modification
Sync types:
```cs
public enum SyncType
{
    // Sync only with character owner
    Local,

    // Sync excluding owner
    Exclude,

    // Broadcast to all
    Broadcast
}
```

Modification examples:
```cs
NetPlayer player;
ICharacterWrapper wrapper = player.Character!;

// Set 400 HP/500 Max HP (broadcast)
wrapper.SetLife(SyncType.Broadcast, 400, 500);

// Modify max HP only (no sync)
wrapper.SetLife(null, null, 500);

// Change current HP only
wrapper.SetLife(null, 400, null);

// Set mana (same pattern as SetLife)
wrapper.SetMana(null, 100, null);

// Set Copper Pickaxe in first slot
wrapper.SetSlot(SyncType.Broadcast, 0, new NetItem(ItemID.CopperPickaxe, 1, 0));

// Set bright red hair color
wrapper.SetColor(SyncType.Broadcast, PlayerColorType.HairColor, new NetColor(255, 0, 0));

// Set 50 completed quests
wrapper.SetQuests(SyncType.Broadcast, 50);
```

# Synchronization
Force synchronization without changes:
```cs
NetPlayer player;
ICharacterWrapper wrapper = player.Character!;

// Sync inventory slot 0
wrapper.SyncSlot(SyncType.Broadcast, 0);

// Sync health
wrapper.SyncLife(SyncType.Broadcast);

// Sync mana
wrapper.SyncMana(SyncType.Broadcast);

// Sync player info (Packet 4)
wrapper.SyncPlayerInfo(SyncType.Broadcast);

// Sync angler quests
wrapper.SyncAngler(SyncType.Broadcast);
```