# Утилиты в NetPlayer.Utils


## Основные свойства
```cs
NetPlayer player;

int tileX = player.Utils.TileX; // X тайла, в котором стоит игрок 
int tileY = player.Utils.TileY; // Y тайла, в котором стоит игрок 

float posX = player.Utils.PosX; // X позиции игрока
float posY = player.Utils.PosY; // Y позиции игрока

Item terrariaItem = player.Utils.HeldItem; // получает экземпляр предмета в руке у игрока.

bool inPvP = player.Utils.InPvP; // включил ли игрок в режиме PvP

bool hasBuff = player.Utils.HasBuff(Terraria.ID.BuffID.ObsidianSkin) // возращает true, если игрок имеет бафф обсидиановой кожи (да, это не относится к свойствам)
```

## Отправка маленькой зоны тайлов

CenteredSquare (куб с определенным значением, где XY это центр):

```cs
NetPlayer player;

player.Utils.SendTileRectangle(x: 100, y: 200, size: 4);

// отправит блоки примерно таким образом:
// ----------
// ----------
// ----XY----
// ----------
// ----------
```

Rectangle:

```cs
player.Utils.SendTileRectangle(x: 100, y: 200, width: 10, height: 4);

// отправит блоки примерно таким образом:
// XY------------------
// --------------------
// --------------------
// --------------------
```