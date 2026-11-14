# События

Пространство имён: `MiniGameBase\event` (+ `event\teamgame`).

Подписка — обычный PMMP `Listener` в своём плагине.

## Ядро и арена

| Событие | Cancellable | API | Когда |
|---------|-------------|-----|-------|
| `MiniGameBaseReadyEvent` | нет | `getMain(): Main` | Ядро готово (тик +1) |
| `PlayerJoinArenaEvent` | да | `getArena()`, `getPlayer()` | Вход в арену |
| `PlayerLeaveArenaEvent` | нет | `getArena()`, `getPlayer()`, `isDisconnect()` | Выход |
| `VoteStartEvent` | нет | `getArena()` | Старт голосования |
| `ArenaGameStartingEvent` | да | `getArena()` | Перед стартом матча |
| `ArenaStartEvent` | нет | `getArena()` | Матч стартовал |
| `ArenaEndEvent` | нет | `getArena()`, `getWinner(): ?Player` | Матч закончен |
| `PlayerDeathInArenaEvent` | да | `getVictim()`, `getKiller()`, `getDeathCause()`, `getPlayer()` deprecated alias | Смерть |

## Фаза и тик (2.1)

| Событие | API | Когда |
|---------|-----|-------|
| `ArenaPhaseChangeEvent` | `getFrom()`, `getTo()` | Смена статуса арены |
| `MatchSecondEvent` | `getElapsedSeconds()` | Каждую секунду в `playing` |

Параллельно вызываются `GameType::onPhaseChange` / `onMatchSecond`.

## Spectator / queue (2.1)

| Событие | Cancellable | API |
|---------|-------------|-----|
| `PlayerEnterMatchSpectatorEvent` | да | `getArena()`, `getPlayer()`, `getReason()` |
| `PlayerLeaveMatchSpectatorEvent` | нет | `getArena()`, `getPlayer()`, `getReason()` |
| `PlayerQueuedEvent` | нет | `getPlayer()`, `getGameType()` |
| `PlayerDequeuedEvent` | нет | `getPlayer()`, `getGameType()`, `getReason()` |

## TeamGame

| Событие | API |
|---------|-----|
| `teamgame\TeamObjectiveBreakEvent` | `getTeam()`, `getPlayer()`, `getX/Y/Z()`, `isOwnTeam()` |
| `teamgame\TeamObjectiveDestroyEvent` | `getTeam()`, `getDestroyer(): ?Player` |

## Примеры

```php
public function onReady(MiniGameBaseReadyEvent $event): void
{
    MiniGameApi::registerGameType('mymode', MyGameType::class);
}

public function onJoin(PlayerJoinArenaEvent $event): void
{
    if (!$event->getPlayer()->hasPermission('mymode.join')) {
        $event->cancel();
    }
}

public function onPhase(ArenaPhaseChangeEvent $event): void
{
    if ($event->getTo() !== 'playing') {
        return;
    }
    // ...
}
```
