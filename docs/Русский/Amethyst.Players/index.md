# Игроки

Основной класс игрока - `NetPlayer`

Основной класс, который отвечает за управление игроками - `Amethyst.Players.PlayerManager`.

Самое основное свойство - Tracker

`PlayerTracker` - класс (свойство `PlayerManager.Tracker`), который хранит в себе экземпляры `NetPlayer`.

Получить экземпляр игрока можно через индексатор:

```cs
int index = 1;
NetPlayer player = PlayerManager.Tracker[index];
```

# Свойства NetPlayer

```cs
// Сетевой ID игрока
public int Index { get; }

// Никнейм игрока
public string Name => _playerName;

// IP-адрес игрока
public string IP { get; set; }

// UUID игрока
public string UUID { get; set; }

// Тип отправителя команд. В данном случае - реальный игрок (SenderType.RealPlayer)
public SenderType Type => SenderType.RealPlayer;

// Ссылка на Terraria.Player
public Player TPlayer => Main.player[Index];

// Интерфейс для сетевого взаимодействия с игроком (например: отправка пакетов)
public INetworkClient Socket => NetworkManager.Provider.GetClient(Index)!;

// Обертка SSC-персонажа для игрока
public ICharacterWrapper? Character { get; internal set; }

// Утилиты для управления игроком
public LocalPlayerUtils Utils { get; }

// Обозначает, что игрок подключен к серверу
public bool IsActive => Socket.IsConnected && !Socket.IsFrozen;

// Обозначает, что игрок подключен к серверу и уже способен выполнять взаимодействия с сервером:
// взаимодействовать с миром, выполнять команды и прочее
public bool IsCapable => IsActive && UUID != "" && Name != "" && _wasSpawned;

// Обозначает, что у игрока есть абсолютно все права
// Этот режим вкючается только через команду /granroot в режиме отладки или сторонними плагинами
public bool IsRootGranted { get; set; }

// Язык игрока. По умолчанию - тот, который вы выбрали в аргументах запуска
// ru-RU, en-US либо дополненные плагинами
public string Language { get; set; }
```

# Получение Terraria.Player

`Player` - класс игрока в Terraria. Не путайте с `NetPlayer` из ядра.

```cs
using Terraria;

NetPlayer player;

Player terrariaPlayer = player.TPlayer;
terrariaPlayer.statLife = 100;
```

# Итерация

`PlayerTracker` наследует `IEnumerable<NetPlayer>`, так что можно использовать `foreach`:

```cs
foreach (NetPlayer player in PlayerManager.Tracker)
{
    Console.WriteLine($"Found player {player.Name} in Tracker");
}
```

Также существуют другие коллекции внутри этого класса, такие как `Capable`, `Alive` и `Dead`:

```cs
foreach (NetPlayer player in PlayerManager.Tracker.Capable)
    Console.WriteLine($"Found capable player: {player.Name}");

foreach (NetPlayer player in PlayerManager.Tracker.Alive)
    Console.WriteLine($"Found alive player: {player.Name}");

foreach (NetPlayer player in PlayerManager.Tracker.Dead)
    Console.WriteLine($"Found dead player: {player.Name}");
```

# Поиск по имени или ID

В `PlayerTracker` есть метод `NetPlayer? FindPlayer(string nameOrIndex)`.

Его можно использовать так:

```cs
NetPlayer? player = PlayerManager.Tracker.FindPlayer("vxlhat") ?? 
                    PlayerManager.Tracker.FindPlayer("VXL") ?? 
                    PlayerManager.Tracker.FindPlayer("vxlh") ?? 
                    PlayerManager.Tracker.FindPlayer("1"); // ID игрока. Но лучше используйте PlayerManager.Tracker[1];

if (player != null)
    Console.WriteLine("vxlhat найден");
```