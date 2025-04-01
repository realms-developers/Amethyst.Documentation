# Tiles (Terraria.Tile)

The core Terraria class for blocks and walls is `Terraria.Tile`.

Tile array is located in `Terraria.Main.tile`:
```cs
using Terraria;

Tile tile = Main.tile[100, 200];
tile.wall = 5; // Replaces wall with ID 5
```

## Tile Fields
```cs
public ushort type;    // Block type ID (starts at 0). 
                       // Only works if tile.active() == true

public ushort wall;    // Wall ID (starts at 1)
public byte liquid;    // Liquid amount
public short frameX;   // Block frame X offset
public short frameY;   // Block frame Y offset
public ushort sTileHeader;  // Compressed short values
public byte bTileHeader;    // Compressed byte values
public byte bTileHeader2;   // Additional compressed values
public byte bTileHeader3;   // Additional compressed values
```

## Tile Properties
```cs
public int collisionType { get; } // Sloped block type
```

## Tile Methods
### Basic Methods
```cs
public bool active();            // Returns if block is active
public void active(bool active); // Sets active state

public bool inActive();          // Returns if deactivated by wires
public void inActive(bool inActive); // Sets wire-deactivated state

public void ClearEverything();   // Clears all data
public void ClearTile();         // Clears block data
public void CopyFrom(Tile _from); // Copies data from another tile
public int blockType();          // Returns sloped block type
public void ResetToType(ushort type); // Resets to specified block
public void ClearMetadata();     // Clears non-block data
public bool halfBrick();         // Half-brick status
public void halfBrick(bool halfBrick);
public byte slope();             // Slope type (0-5)
public void slope(byte slope);
public bool HasSameSlope(Tile tile); // Compares slopes
```

### Liquid Handling
```cs
public byte liquidType();        // Liquid ID (0=Water,1=Lava,2=Honey,3=Shimmer)
public void liquidType(int liquidType);

public bool lava();              // Checks for lava
public void lava(bool lava);

public bool honey();             // Checks for honey
public void honey(bool honey);

public bool shimmer();           // Checks for shimmer
public void shimmer(bool shimmer);
```

### Paint & Coatings
```cs
public byte color();             // Block paint ID
public void color(byte color);

public byte wallColor();         // Wall paint ID
public void wallColor(byte wallColor);

public bool invisibleBlock();    // Echo Coating status for block
public void invisibleBlock(bool invisibleBlock);

public bool invisibleWall();     // Echo Coating status for wall
public void invisibleWall(bool invisibleWall);

public void ClearBlockPaintAndCoating(); // Removes block paint/coating
public void ClearWallPaintAndCoating();  // Removes wall paint/coating
```

### Wires
```cs
public bool wire4();             // Yellow wire status
public void wire4(bool wire4);
// Similar methods: wire(), wire2(), wire3()
```