# Terraria.ID

The `TerrariaAPI.dll` contains the `Terraria.ID` namespace, which can be used in plugins.

Example using `Terraria.ID.LiquidID`:
```cs
namespace Terraria.ID;

public static class LiquidID
{
    public const short Water = 0;
    public const short Lava = 1;
    public const short Honey = 2;
    public const short Shimmer = 3;
    public static readonly short Count = 4;
}
```

Other important classes include:

- `ItemID`
- `NPCID`
- `BuffID`
- `TileID`
- `WallID`
- `PaintID`
- `PaintCoatingID`
- `ProjectileID`
- `InvasionID`
- `GameModeID`
- `PlayerSlotID`
- `MessageID` (packet IDs, though not all are documented)