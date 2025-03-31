# Terraria.ID

В `TerrariaAPI.dll` присутствует пространство имен `Terraria.ID`, которое может быть использовано в плагинах.

На примере `Terraria.ID.LiquidID`:

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

Существуют и другие классы, но наиболее важные из них это:

- `ItemID`
- `NPCID`
- `BuffID`
- `TileID`
- `WallID`
- `PaintID`
- `PaintCoatingID`
- `ProjecitleID`
- `InvasionID`
- `GameModeID`
- `PlayerSlotID`
- `MessageID` (ID пакетов, но в большинстве случаев не все подписанные)