# Тайлы (Terraria.Tile)

Основной класс в Terraria, который отвечает за блоки и стены - `Terraria.Tile`:

Массив тайлов находится в `Terraria.Main.tile`:

```cs
using Terraria;

Tile tile = Main.tile[100, 200];

tile.wall = 5; // заменяет стену на 5 (ID)
```

## Поля Tile

Учтите, что существует также состояние `active` у блока.

```cs
public ushort type; // тип блока, ID начинается с 0
                    // не будет работать или отображаться, если tile.active() == false

public ushort wall; // wall, ID начинается с 1

public byte liquid; // количество жидкости

public short frameX; // смещение кадра блока по X

public short frameY; // смещение кадра блока по Y

public ushort sTileHeader; // другие сжатые значения по short

public byte bTileHeader; // другие сжатые значения по byte

public byte bTileHeader2; // другие сжатые значения по byte

public byte bTileHeader3; // другие сжатые значения по byte
```

## Свойства Tile

```cs
public int collisionType { get; } // указывает на тип полублока
```

## Методы Tile

### Базовые

```cs
// возвращает bool, является ли блок активным
public bool active();
// устанавливает состояние активности
public void active(bool active);


// возвращает bool, является ли блок деактивированным проводами
public bool inActive();
// устанавливает состояние деактивированности проводами
public void inActive(bool inActive);

// очищает все данные
public void ClearEverything();

// очищает данные блока (тип полублока, деактивацию проводами,)
public void ClearTile();

// копирует данные с другого блока
// используйте этот метод, так как прямое использование newTile = oldTile, приведет к тому, что newTile будет ссылкой на oldTile
public void CopyFrom(Tile _from);

// возвращает тип полублока как int
public int blockType();

// удаляет все данные, и заменяет блок на указанный
public void ResetToType(ushort type);

// удаляет все данные помимо блока
internal void ClearMetadata();

public bool halfBrick();
public void halfBrick(bool halfBrick);

public byte slope();
public void slope(byte slope);

// возвращает bool, является ли тип полублока определенным
public bool topSlope();
public bool bottomSlope();
public bool leftSlope();
public bool rightSlope();

// возвращает bool, является ли тип полублока таким же как у 'tile'
public bool HasSameSlope(Tile tile);
```

### Жидкости

```cs
// возвращает ID жидкости
public byte liquidType();
// устанавливает ID жидкости
public void liquidType(int liquidType);


// возвращает bool, имеет ли блок в себе жидкость лавы
public bool lava();
// устанавливает жидкость лавы
public void lava(bool lava);


// возвращает bool, имеет ли блок в себе жидкость меда
public bool honey();
// устанавливает жидкость меда
public void honey(bool honey);


// возвращает bool, имеет ли блок в себе жидкость мерцания
public bool shimmer();
// устанавливает жидкость мерцания
public void shimmer(bool shimmer);
```

### Краски и покрытия

```cs
// возращает ID краски блока
public byte color();
// устанавливает краску блока
public void color(byte color);


// возращает ID краски стены
public byte wallColor();
// устанавливает краску стены
public void wallColor(byte wallColor);


// возвращает bool, если блок имеет эхо-покрытие
public bool invisibleBlock();
// устанавливает эхо-краску для блока
public void invisibleBlock(bool invisibleBlock);


// возвращает bool, если стена имеет эхо-покрытие
public bool invisibleWall();
// устанавливает эхо-краску для стены
public void invisibleWall(bool invisibleWall);

// очищает краску и покрытие для блока
public void ClearBlockPaintAndCoating();
// очищает краску и покрытие для стены
public void ClearWallPaintAndCoating();
```

### Провода

```cs
// 4 ID это вроде желтый, я не помню :(
// возвращает bool, имеется ли тут провод с 4 ID.
public bool wire4();
// устанавливает провод с 4 ID.
public void wire4(bool wire4);


// все также с wire(), wire2(), wire3()
```