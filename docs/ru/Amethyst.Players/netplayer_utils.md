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

## Отправка большой зоны тайлов (чанки/секции)

Terraria делит мир на секции (также как и Minecraft на чанки), одна секция содержит в себе 200 блоков в ширину и 150 в высоту.

Для получения позиции секции можно использовать класс `Netplay`:

```cs
int tileX = 2000;
int tileY = 1500;

int sectionX = Netplay.GetSectionX(tileX);
int sectionY = Netplay.GetSectionY(tileY);
```

Код отправки секций:

```cs
NetPlayer player;

// рассчитывает какие секции отправить с XY 1130:130 по XY 1643:654 
// и отправляет их игроку
player.SendMassTiles(1130, 130, 1643, 654);

// отправляет секцию игроку
player.SendSection(Netplay.GetSectionX(3453), Netplay.GetSectionY(546));

// отправляет секцию игроку, если игрок до этого не получал ее
player.RequestSendSection(Netplay.GetSectionX(3453), Netplay.GetSectionY(546));
```

## Статус-бар

Наверняка вы видели на Terraria Realms или Penguin Games текст справа под мини-картой. Мы это называем статус-баром.

Вот как его использовать:

```cs
NetPlayer player;

// отправит игроку текст статус-бара
// текст будет отображаться под мини-картой, т.к. здесь padding = true
player.SendStatusText("My text under minimap", padding: true);

// отправит игроку текст статус-бара
// текст будет отображаться под количеством здоровья, т.к. здесь padding = false
player.SendStatusText("My text under healthbar", padding: false);
```

## Телепортация

```cs
NetPlayer player;

// телепортирует игрока по тайл-координате 100:500
player.TeleportTile(100, 500);

// телепортирует игрока по координате 3200f:1600f (200:100 по тайл-координате)
player.Teleport(3200f, 1600f);
```

## Прочее

```cs
NetPlayer player;

// выдает игроку 1x 'Клинок Земли'
player.GiveItem(Terraria.ID.ItemID.TerraBlade, 1, 0);

// ударяет игрока на 300 единиц.
// если игрок умрет после этого, то в чате 
// высветится причина смерти "Custom death reason text"
player.Hurt(300, "Custom death reason text");

// если игрок умрет после этого, то в чате 
// высветится причина смерти "<Игрок> was died."
player.Hurt(300, PlayerDeathReason.LegacyDefault());


// убивает игрока, в чате высвечивается "<Игрок> был эпично уничтожен!"
player.Kill($"{player.Name} был эпично уничтожен!");

// убивает игрока, в чате высвечивается "<Игрок> was died."
player.Kill();
player.Kill(PlayerDeathReason.LegacyDefault());


// выдает бафф регенерации игроку на 1 час, 5 минут, 15 секунд
player.AddBuff(Terraria.ID.BuffID.Regeneration, new TimeSpan(hours: 1, minutes: 5, seconds: 15));
// если вам нравится заниматься ерундой, или у вас на душе осадок после TShock, 
// вы можете использовать метод так:
player.AddBuff(Terraria.ID.BuffID.Regeneration, 234900);
```