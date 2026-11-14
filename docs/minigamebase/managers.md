# Менеджеры

Доступ: `MiniGameApi::*()` или `MiniGameApi::main()?->get*()`.

Полные сигнатуры — в [Справочнике](./reference).

## ArenaManager — `arenaManager()`

Создание арен, join/leave, reconnect, персист `arenas.yml`, метрики.

Ключевые методы: `createNewArena`, `addPlayerToArena`, `removePlayerFromArena`, `getPlayerArena`, `findArenaByGameType`, `findAvailableArena`, `tryReconnectPlayer`, `getWaitingArenas` / `getLobbyArenas` / `getActiveArenas`, `savePersistentState`, `stopAllArenas`.

## GameTypeManager — `gameTypes()`

`registerGameType`, `getGameType`, `isRegistered`, `unregisterGameType`, `getAllGameTypeInstances`, `getGameTypesInfo`.

## TimerManager — `timers()`

| Метод | Назначение |
|-------|------------|
| `startLobbyTimer($arenaId, $sec, $cb)` | Countdown лобби |
| `startVoteTimer(...)` | Голосование |
| `startGameTimer(...)` | Длительность матча |
| `startRepeatingTimer($id, $periodSec, $cb)` | Повтор |
| `stopArenaTimers` / `stopTimer` / `stopAllTimers` / `clearAllTimers` | Стоп |
| `isTimerActive` / `getRemainingTime` / `getActiveTimersCount` | Статус |

## FormManager — `forms()`

`sendForm`, `sendDduiForm`, `hasOpenForm`, `closeForm`, `createFormTitle`, `getStatusText`, `registerMenuAction`, `menuActions()`, `getStats`.

Готовые UI: `MainMenuForm`, `JoinGameForm`, `VoteMapForm`, `StatsForm`, `TopPlayersForm`, …

## WorldManager

`createGameInstance($template, $instanceName)`, `getGameInstance`, `unloadGameInstance`, `removeGameInstance`, `clearAllGameInstances`, `getWorldByName`.

Копии `*_unixTime` можно удалять; шаблоны миров — нет.

## LobbyManager

`teleportToLobby(Player)`, `prepareLobbyPlayer(Player)`.

## StatisticsManager

`updatePlayerStat`, `getPlayerStats`, `getTopPlayers`, `saveAll` — поверх `IDataStorage`.

## CommandManager — `commands()`

```php
MiniGameApi::commands()?->register(
    $plugin, 'mygame', 'desc', '/mygame', ['alias'],
    'mygame.use',
    function ($sender, string $label, array $args): bool { return true; },
    playerOnly: true
);
```

Также: `registerCommand`, `unregister`, `isRegistered`, `get`.

## ModuleManager

`getModule($id)`, `isModuleLoaded`, `getLoadedModules`, `getModulesInfo`, `loadModule`, `unloadModule`, `registerModule`, `broadcastEvent`.

## IDataStorage / UniversalDataStorage — `storage()`

См. [Storage](./storage).
