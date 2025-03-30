# SSC

SSC - режим сервера, при котором персонаж игроков сохраняется на сервере.

Узнать, включен ли SSC можно свойством `bool PlayerManager.IsSSCEnabled`

Персонаж игрока хранится в `ICharacterWrapper? NetPlayer.Character`

Разберем основные свойства ICharacterWrapper:

```cs
// Модель базы данных персонажа.
public CharacterModel Model { get; }

// Означает, что на сервере включен режим SSC и игрок уже может принимать пакеты SSC
public bool SupportsChange { get; }
    
// Означает, что игроку заблокирована возможность изменять персонажа.
// Только плагины/модули/ядро может изменять персонажа.
public bool IsReadonly { get; set; }
```

# Индексаторы

У `ICharacterWrapper` есть два индексатора:

Получение слотов по индексу:

```cs
NetPlayer player;
ICharacterWrapper wrapper = player.Character!;

NetItem item = wrapper[5] // Получает предмет в 6 слоту (не 5, потому что слот с индексом 0 это первый слот в привычном понимании.)
```

Получение цвета чего-либо по индексу:

```cs
NetPlayer player;
ICharacterWrapper wrapper = player.Character!;

NetColor hairCol = wrapper[PlayerColorType.HairColor];
NetColor skinCol = wrapper[PlayerColorType.SkinColor];
```

# Изменение

Для начала стоит понять типы синхронизаций (`SyncType`):

```cs
public enum SyncType
{
    // синхронизация только с владельцем персонажа
    Local,

    // синхронизация с пропуском владельца персонажа
    Exclude,

    // синхронизация со всеми
    Broadcast
}
```

```cs
NetPlayer player;
ICharacterWrapper wrapper = player.Character!;

// установит игроку 400 здоровья, 500 максимального здоровья
// Broadcast огласит это всем игрокам.
wrapper.SetLife(SyncType.Broadcast, 400, 500);

// изменит только максимальное здоровье
// null-синхронизация не будет никому отправлять пакеты.
wrapper.SetLife(null, null, 500);

// изменит только текущее здоровье
wrapper.SetLife(null, 400, null);

// работает так же как и SetLife, но с маной
wrapper.SetMana(null, 100, null);

// установит в первом слоте инвентария медную кирку
wrapper.SetSlot(SyncType.Broadcast, 0, new NetItem(Terraria.ID.ItemID.CopperPickaxe, 1, 0));

// установит ярко-красный цвет волос игроку
wrapper.SetColor(SyncType.Broadcast, PlayerColorType.HairColor, new NetColor(255, 0, 0));

// установит 50 выполненных квестов игроку
wrapper.SetQuests(SyncType.Broadcast, 50);
```

# Синхронизация

Если вам нужно синхронизировать персонажа без изменений, то к вашим услугам методы ниже:

```cs

NetPlayer player;
ICharacterWrapper wrapper = player.Character!;

// синхронизирует первый слот инвентаря
wrapper.SyncSlot(SyncType.Broadcast, 0);

// синхронизирует здоровье
wrapper.SyncLife(SyncType.Broadcast);

// синхронизирует ману
wrapper.SyncMana(SyncType.Broadcast);

// синхронизирует информацию об игроке (4 пакет)
wrapper.SyncPlayerInfo(SyncType.Broadcast);

// синхронизирует квесты
wrapper.SyncAngler(SyncType.Broadcast);
```