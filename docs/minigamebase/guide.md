# Быстрый старт

Своя мини-игра на MiniGameBase за 5 шагов.

## 1. Зависимость

`plugin.yml`:

```yaml
name: MyMiniGame
main: MyMiniGame\Loader
api: 5.0.0
depend: [MiniGameBase]
```

## 2. Класс GameType

```php
<?php
declare(strict_types=1);

namespace MyMiniGame;

use MiniGameBase\manager\Arena;
use MiniGameBase\types\GameType;
use pocketmine\player\Player;

final class MyGameType extends GameType
{
    public function onGameStart(Arena $arena): void
    {
        foreach ($arena->getPlayers() as $player) {
            $this->preparePlayer($arena, $player);
        }
    }

    public function onGameEnd(Arena $arena, ?Player $winner = null): void {}

    public function preparePlayer(Arena $arena, Player $player): void
    {
        $player->getInventory()->clearAll();
    }

    public function onPlayerJoin(Arena $arena, Player $player): void {}
    public function onPlayerLeave(Arena $arena, Player $player): void {}

    public function onPlayerDeath(
        Arena $arena,
        Player $player,
        ?Player $killer = null,
        string $deathCause = 'unknown'
    ): void {
        // победа / spectator
    }

    public function getRecommendedArenaSettings(): array
    {
        return [
            'min_players' => 2,
            'max_players' => 8,
            'pvp_enabled' => true,
            'damage_enabled' => true,
            'hunger_enabled' => false,
            'fall_damage' => true,
            'block_break' => false,
            'block_place' => false,
        ];
    }
}
```

Эталон в репозитории: `examples/ExampleGameType.php`, `DefaultGameType`.

## 3. Регистрация после Ready

**Не** регистрируйте тип синхронно в своём `onEnable()`: MiniGameBase шлёт `MiniGameBaseReadyEvent` на **следующий тик**.

```php
use MiniGameBase\api\MiniGameApi;
use MiniGameBase\event\MiniGameBaseReadyEvent;
use pocketmine\event\Listener;

final class ReadySubscriber implements Listener
{
    public function onReady(MiniGameBaseReadyEvent $event): void
    {
        MiniGameApi::registerGameType('mymode', MyGameType::class);
    }
}
```

В `Loader::onEnable()`:

```php
$this->getServer()->getPluginManager()->registerEvents(new ReadySubscriber(), $this);
```

## 4. Шаблон арены

`plugin_data/MiniGameBase/config.yml`:

```yaml
arena_templates:
  mymode:
    name_template: "Мой режим {number}"
    available_maps: ["map_1"]
    min_players: 2
    max_players: 8
    game_type: "mymode"

default_maps:
  map_1:
    name: "Первая карта"
    world: "map_1"
    description: "Описание"
    layout:
      spawn: [0, 64, 0]
      center: {x: 0, y: 70, z: 0, yaw: 90}
```

Папка `worlds/map_1` должна существовать. Ключ `game_type` = id из `registerGameType`.

## 5. Вход игроков

| Способ | Как |
|--------|-----|
| Игрок | `/mg join` или `/mg join mymode` |
| Админ | `/mg arena create mymode` |
| Код | `MiniGameApi::arenaManager()?->addPlayerToArena($player)` |
| Матчмейкинг | `/mg join mymode` при отсутствии свободной арены → очередь |

Поток: **waiting → voting → lobby → playing → finished → waiting**.

## Дальше

- Хуки фазы/тика: [GameType](./gametype)
- HUD / зоны / spectator: [Сервисы](./services)
- Полный фасад: [API](./api)
