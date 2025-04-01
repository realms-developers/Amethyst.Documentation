# Utilities in NetPlayer.Utils

## Key Properties
```cs
NetPlayer player;

int tileX = player.Utils.TileX; // X coordinate of the tile the player is standing on
int tileY = player.Utils.TileY; // Y coordinate of the tile the player is standing on

float posX = player.Utils.PosX; // Player's X position
float posY = player.Utils.PosY; // Player's Y position

Item terrariaItem = player.Utils.HeldItem; // Gets the item instance held by the player

bool inPvP = player.Utils.InPvP; // Whether the player has PvP enabled

bool hasBuff = player.Utils.HasBuff(Terraria.ID.BuffID.ObsidianSkin); // Returns true if the player has the Obsidian Skin buff
```

## Sending Small Tile Areas

**Centered Square** (a square centered at XY coordinates):
```cs
NetPlayer player;

player.Utils.SendTileRectangle(x: 100, y: 200, size: 4);

// Sends tiles in this pattern:
// ----------
// ----------
// ----XY----
// ----------
// ----------
```

**Rectangle**:
```cs
player.Utils.SendTileRectangle(x: 100, y: 200, width: 10, height: 4);

// Sends tiles in this pattern:
// XY------------------
// --------------------
// --------------------
// --------------------
```

## Sending Large Tile Areas (Chunks/Sections)

Terraria divides the world into sections (like Minecraft chunks), each containing 200 blocks in width and 150 in height.

Get section coordinates using `Netplay`:
```cs
int tileX = 2000;
int tileY = 1500;

int sectionX = Netplay.GetSectionX(tileX);
int sectionY = Netplay.GetSectionY(tileY);
```

Sending sections:
```cs
NetPlayer player;

// Sends sections covering tile coordinates 1130:130 to 1643:654
player.SendMassTiles(1130, 130, 1643, 654);

// Sends a specific section
player.SendSection(Netplay.GetSectionX(3453), Netplay.GetSectionY(546));

// Sends a section only if the player hasn't received it
player.RequestSendSection(Netplay.GetSectionX(3453), Netplay.GetSectionY(546));
```

## Status Bar

Display text under the minimap or healthbar:
```cs
NetPlayer player;

// Sends text under the minimap (padding: true)
player.SendStatusText("My text under minimap", padding: true);

// Sends text under the healthbar (padding: false)
player.SendStatusText("My text under healthbar", padding: false);
```

## Teleportation
```cs
NetPlayer player;

// Teleport to tile coordinates (100, 500)
player.TeleportTile(100, 500);

// Teleport to world coordinates (3200f, 1600f) (tile 200:100)
player.Teleport(3200f, 1600f);
```

## Miscellaneous Actions
```cs
NetPlayer player;

// Give 1x Terra Blade
player.GiveItem(Terraria.ID.ItemID.TerraBlade, 1, 0);

// Deal 300 damage with custom death message
player.Hurt(300, "Custom death reason text");

// Deal 300 damage with default death message
player.Hurt(300, PlayerDeathReason.LegacyDefault());

// Kill player with custom message
player.Kill($"{player.Name} was epicly destroyed!");

// Kill player with default message
player.Kill();

// Add Regeneration buff for 1h 5m 15s
player.AddBuff(Terraria.ID.BuffID.Regeneration, new TimeSpan(hours: 1, minutes: 5, seconds: 15));

// Alternative method (ticks)
player.AddBuff(Terraria.ID.BuffID.Regeneration, 234900);
```