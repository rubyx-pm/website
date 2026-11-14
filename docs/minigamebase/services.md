# Сервисы 2.1

Все сервисы живут в `Main` и доступны через `MiniGameApi::*()`.

## HudService — `hud()`

Единый UX: scoreboard / bossbar / actionbar / tip.

| Метод | Описание |
|-------|----------|
| `showScoreboard(Player, string $template): bool` | Показать (через модуль scoreboard) |
| `clearScoreboard(Player)` | |
| `showBossBar(Player, string $title, float $progress = 1.0, int $color = 5): ?int` | |
| `updateBossBar(Player, ?string $title, ?float $progress, ?int $color): bool` | |
| `clearBossBar(Player)` | |
| `actionBar(Player, string $text)` | |
| `tip(Player, string $text)` | |
| `clearAll(Player)` | Сбросить всё HUD |

## MatchSpectatorService — `matchSpectators()`

In-match spectator: игрок **остаётся** в мире матча (смерть / observe).

| Метод | Описание |
|-------|----------|
| `enter(Player, Arena, string $reason = 'died', bool $hideFromPlayers = true): bool` | Вход; шлёт cancellable event |
| `leave(Player, string $reason = 'leave'): bool` | Выход |
| `isSpectator(string $playerName): bool` | |
| `getArenaId(string $playerName): ?string` | |
| `getRespawnPosition(Player): ?Position` | |
| `listArenaSpectatorNames(string $arenaId): array` | |
| `clearArena(string $arenaId)` | Teardown матча |
| `onQuit(Player)` | Cleanup |

События: `PlayerEnterMatchSpectatorEvent` (cancellable), `PlayerLeaveMatchSpectatorEvent`.  
Хук: `GameType::onMatchSpectatorEnter`.

## SpectatorService — `spectators()`

Hub-spectator: смотреть арену **извне**, игрок уходит из состава арены.

| Метод | Описание |
|-------|----------|
| `enter(Player, Arena): bool` | |
| `leave(Player): bool` | |
| `isSpectator` / `getArenaId` / `onQuit` | |

Команда: `/mg spectate [arenaId]|leave`.

## MovementLockService — `movementLocks()`

Hard-lock: immobile + якорь + TTL в тиках сервера.

```php
MiniGameApi::movementLocks()?->lock(
    $player,
    anchor: $pos,          // Vector3|null — иначе текущая позиция
    arenaId: $arena->getId(),
    ttlTicks: 40,          // null = без TTL
    immobile: true
);
```

| Метод | Описание |
|-------|----------|
| `lock(Player, ?Vector3 $anchor, ?string $arenaId, ?int $ttlTicks, bool $immobile = true)` | |
| `unlock(Player)` | Снять + восстановить mobile |
| `isLocked(string $name): bool` | |
| `getAnchor(string $name): ?Position` | |
| `tick()` | TTL / enforce (Main) |
| `enforce(Player)` | Подтянуть к якорю |
| `clearArena(?string $arenaId)` | Teardown |
| `onQuit(Player)` | |

Не телепортировать каждый тик вручную — сервис сам enforce'ит якорь.

## ZoneService — `zones()`

Убывающая / фиксированная damage-зона (border, evacuation).

```php
$zones = MiniGameApi::zones();
$zones?->startShrink('evac', $arena, $center, 80.0, 20.0, 120, 2.0);
$zones?->setFixed('safe', $arena, $center, 15.0, 1.0);
$zones?->isInside('evac', $player->getPosition());
// в onMatchSecond:
$zones?->tickSecond($arena);
```

| Метод | Описание |
|-------|----------|
| `startShrink($id, Arena, Vector3 $center, float $startR, float $endR, int $durationSec, float $dmgPerSec = 1.0)` | Линейный shrink |
| `setFixed($id, Arena, Vector3 $center, float $radius, float $dmgPerSec = 1.0)` | Фикс. радиус |
| `stop(string $zoneId)` | |
| `clearArena(string $arenaId)` | |
| `getRadius(string $zoneId): ?float` | Текущий радиус |
| `isInside(string $zoneId, Vector3 $pos): ?bool` | `null` если зоны нет |
| `tickSecond(Arena)` | Урон вне радиуса (не spectator / creative) |

## LayoutCatalog — `layouts()`

Именованные точки из `default_maps.<id>.layout`.

Формат точки: `[x,y,z]` или `{x,y,z,yaw?,pitch?}`.

| Метод | Описание |
|-------|----------|
| `get(Arena, string $pointId): ?Location` | |
| `all(Arena): array` | pointId → Location |
| `teleport(Player, Arena, string $pointId): bool` | |
| `has(Arena, string $pointId): bool` | |

## MatchmakingService — `matchmaking()`

| Метод | Описание |
|-------|----------|
| `enqueue(Player, string $gameType = ''): bool` | В очередь |
| `dequeue(Player, string $reason = 'leave')` | |
| `isQueued(string $name): bool` | |
| `tick()` | Подбор / таймаут |

События: `PlayerQueuedEvent`, `PlayerDequeuedEvent`.  
Конфиг: `matchmaking.max_queue_seconds`, `fill_weight`.  
Команды: `/mg join [gameType]`, `/mg queue`.

## PartyManager — `parties()`

| Метод | Описание |
|-------|----------|
| `create(Player $leader): bool` | |
| `invite(Player $leader, string $targetName): bool` | |
| `accept(Player): bool` | |
| `leave(Player)` | |
| `getMembers` / `getLeader` / `size` / `isLeader` | |

Команда: `/mg party create|invite|accept|leave|info`.

## Прочие сервисы Main

Не все вынесены в фасад — доступ через `MiniGameApi::main()`.

| Сервис | Методы | Назначение |
|--------|--------|------------|
| `ArenaJoinService` | `tryJoin(Player, ?arenaId)` | Join + сообщения |
| `HubStatusService` | `tick()` | Хаб scoreboard/status |
| `DoctorService` | `run(CommandSender)` | `/mg doctor` |
| `SeasonService` | `currentId`, `statKey`, `record`, `info`, `startNew` | Сезоны статы |
| `AchievementService` | `evaluatePlayer(Player)` | Правила из конфига |
| `StatsExporter` | `export`, `tick` | Webhook export |
| `ArenaPoolScheduler` | `tick()` | Расписание пула арен |

## Типичный teardown матча

```php
public function onGameEnd(Arena $arena, ?Player $winner = null): void
{
    $id = $arena->getId();
    MiniGameApi::zones()?->clearArena($id);
    MiniGameApi::movementLocks()?->clearArena($id);
    MiniGameApi::matchSpectators()?->clearArena($id);
    MiniGameApi::layouts(); // points are map-bound, no clear needed
}
```
