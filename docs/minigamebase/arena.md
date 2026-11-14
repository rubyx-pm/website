# Арены

## Жизненный цикл

```
waiting → voting → lobby → playing → finished → (reset) waiting
```

| Статус | Смысл |
|--------|--------|
| `waiting` | Набор игроков |
| `voting` | Голосование за карту |
| `lobby` | Countdown перед стартом |
| `playing` | Матч |
| `finished` | Конец (далее сброс) |

Смена фазы: `ArenaPhaseChangeEvent` + `GameType::onPhaseChange`.

## Arena — ключевые методы

Класс: `MiniGameBase\manager\Arena`.

### Игроки и join

| Метод | Описание |
|-------|----------|
| `addPlayer` / `removePlayer` | Вход / выход |
| `canJoin` / `canPlayerJoin` / `getPlayerJoinDenialReason` | Можно ли войти |
| `isPlayerInArena` | |
| `getPlayers` / `getDeadPlayers` / `getAlivePlayersCount` / `getDeadPlayersCount` | Составы |
| `addDeadPlayer` / `isPlayerDead` | Мёртвые |
| `getPlayerCount` / `getMaxPlayers` / `getMinPlayers` | Лимиты |

Константы отказа: `JOIN_DENIAL_MATCH_IN_PROGRESS` (= 2), `JOIN_DENIAL_ARENA_FULL` (= 3).

### Матч

| Метод | Описание |
|-------|----------|
| `startGame` / `stopGame(?Player $winner)` | Старт / стоп |
| `forceStartGameForTest(?string $mapId)` | Админ/debug |
| `handlePlayerDeath` | Смерть → событие + GameType |
| `isGameRunning` / `isInLobby` / `isVoting` | Флаги |
| `getStatus` / `getId` / `getName` | Мета |
| `getGameType` / `getGameMode` / `setGameMode` | Тип режима |
| `getGameWorld(): ?World` | Мир копии |
| `getCurrentMap` / `getAvailableMaps` | Карты |
| `getGameStartTime` / `getGameTime` | Время |
| `tick` / `tickSecond` | Тики (из ArenaManager / Main) |

### Vote / UX / reconnect

| Метод | Описание |
|-------|----------|
| `voteForMap` / `showVoteFormToPlayers` / `sendVoteFormToPlayer` | Голосование |
| `getVotes` / `addVote` / `clearVotes` / `setVoting` | |
| `broadcastSound` / `sendGameInfoActionBar` | UX |
| `markDisconnected` / `tryReconnect` / `getDisconnectedPlayerNames` | Reconnect |
| `processDisconnectedTimeouts` / `checkStopGameIfNoParticipantsRemain` | Таймауты |
| `moveToLobby` / `moveToWaiting` | Смена фазы |
| `setPendingVoidKiller` / `getAndClearPendingVoidKiller` | Void kill attribution |

## ArenaManager

Фасад: `MiniGameApi::arenaManager()`.

| Метод | Описание |
|-------|----------|
| `createNewArena(?string $templateId = null): ?Arena` | По шаблону |
| `addPlayerToArena(Player, ?string $arenaId = null): bool` | Join |
| `removePlayerFromArena(Player): bool` | Leave |
| `getArena` / `getAllArenas` / `getPlayerArena` | Поиск |
| `findAvailableArena` / `findArenaByGameType` / `getBestArenaForBalance` | Подбор |
| `getArenasByStatus` / `getWaitingArenas` / `getLobbyArenas` / `getActiveArenas` | Фильтры |
| `tryReconnectPlayer(Player): bool` | Вернуть в матч |
| `removeEmptyArena` / `cleanupEmptyArenas` / `clearAll` / `stopAllArenas` | Обслуживание |
| `getArenasStats` / `getOverallStats` / `getPerformanceStats` | Метрики |
| `savePersistentState` / `loadArenasFromDisk` | `arenas.yml` |
| `processExpiredReconnects` / `tick` | Периодика |
| `formatArenasList` / `formatArenaInfo` / `getStatusText` | UI-хелперы |

Удобный join с сообщениями: `ArenaJoinService::tryJoin` (команда `/mg join`).

## Reconnect

```yaml
game:
  reconnect_enabled: true
  reconnect_timeout: 30   # сек; 0 = выкл
```

1. Дисконнект в `playing` → `markDisconnected` → слот в `getDisconnectedPlayerNames()`.
2. `GameType::onPlayerDisconnectedDuringGame`.
3. Повторный вход → `tryReconnectPlayer`.
4. Таймаут → `processExpiredReconnects`; пустая арена → стоп матча.
