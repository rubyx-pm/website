# Storage, Forms, Worlds, PlaceholderAPI

## IDataStorage — `storage()`

Реализация: `UniversalDataStorage` (MySQL).

```php
$s = MiniGameApi::storage();
$s?->incrementPlayerData($player, 'wins', 1);
$wins = $s?->getPlayerData($player, 'wins', 0);
$top = $s?->getTopPlayers('wins', 10);
```

| Метод | Назначение |
|-------|------------|
| `getPlayerData` / `setPlayerData` / `incrementPlayerData` | Online |
| `getPlayerDataByName` / `setPlayerDataByName` | В т.ч. офлайн |
| `getAllPlayerData` / `getPlayerDataKeys` / `hasPlayerData` | Обзор |
| `getTopPlayers` / `getPlayersCount` / `getKeyStatistics` | Агрегаты |
| `deletePlayerData` / `clearPlayerData` | Удаление |
| `getPlayerStats` / `updatePlayerStats` / `setPlayerStats` | Стат-ключи |
| `clearPlayerCache` / `clearAllCache` / `saveAll` | Кэш / flush |

Типичные ключи: `wins`, `kills`, `deaths`, `games`. Сезонные — через `SeasonService::statKey('wins')`.

### StatisticsManager

Обёртка: `updatePlayerStat`, `getPlayerStats`, `getTopPlayers`, `saveAll` — `Main::getStatisticsManager()`.

## Forms — `forms()`

`FormManager`: cooldown и защита от двойного открытия.

| Метод | Описание |
|-------|----------|
| `sendForm(Player, Form, bool $forceOpen = false): bool` | Bedrock forms |
| `sendDduiForm(Player, DduiForm, bool $forceOpen = false): bool` | DDUI (Rubyx) |
| `hasOpenForm` / `closeForm` | Сессия |
| `createFormTitle` / `getStatusText` | Стиль / локаль статуса |
| `registerMenuAction` / `menuActions()` | Кнопки MainMenu |
| `getStats(): array` | Метрики |

Готовые UI: `MainMenuForm`, `JoinGameForm`, `JoinExistingArenaForm`, `CreateNewArenaForm`, `AllArenasForm`, `ArenaInfoForm`, `StatsForm`, `OverallStatsForm`, `TopPlayersForm`, `TopByStatForm`, `VoteMapForm`.

Базовые типы: `SimpleForm`, `CustomForm`, `ModalForm`.

```php
MiniGameApi::registerMenuAction('mymode_shop', 'Магазин режима', function (Player $p): void {
    // open shop
}, 'mymode.shop');
```

## Таймеры — `timers()`

| Метод | Описание |
|-------|----------|
| `startLobbyTimer` / `startVoteTimer` / `startGameTimer` | Привязка к арене |
| `startRepeatingTimer` | Повтор |
| `stopArenaTimers` / `stopAllTimers` / `stopTimer` / `clearAllTimers` | Стоп |
| `isTimerActive` / `getRemainingTime` / `getActiveTimersCount` | Статус |

Scheduler плагина: `MiniGameApi::scheduleDelayed` / `scheduleRepeating`.

## Миры — WorldManager

```php
$wm = MiniGameApi::main()?->getWorldManager();
$world = $wm?->createGameInstance('map_1', 'map_1_' . time());
```

| Метод | Описание |
|-------|----------|
| `createGameInstance(string $templateWorld, string $instanceName): ?World` | Copy-on-play |
| `getGameInstance` / `getWorldByName` / `getAllGameInstances` | Поиск |
| `unloadGameInstance` / `removeGameInstance` | Unload + удаление копии |
| `clearAllGameInstances` | Полная очистка |

Имена копий вида `*_unixTime` — только их безопасно удалять. Шаблоны (`lobby`, `dbd_1`, …) не трогать.

Хелперы: `utils\WorldCopyPolicy`, `utils\FsTree`.

## LobbyManager

| Метод | Описание |
|-------|----------|
| `teleportToLobby(Player)` | ТП в хаб |
| `prepareLobbyPlayer(Player)` | Сброс состояния |

## PlaceholderAPI

Опционально (`NetherByte\PlaceholderAPI`):

| Плейсхолдер | Смысл |
|-------------|--------|
| `%minigame_wins%` | Победы |
| `%minigame_kills%` | Убийства |
| `%minigame_deaths%` | Смерти |
| `%minigame_games%` | Игр сыграно |
| `%minigame_arena%` | Текущая арена |
| `%minigame_players%` | Игроков в арене |
| `%minigame_time%` | Время |

## Анти-паттерны

1. Регистрация `GameType` в `onEnable` без `ReadyEvent`.
2. Удаление папок миров-шаблонов вручную.
3. Sleep / долгая работа на server thread.
4. Дублирование lifecycle матча вне `Arena`.
5. Жёсткая зависимость от auth/economy без проверки модуля.
6. Игнор `null` у `MiniGameApi::*()` до Ready.
7. Teleport каждый тик вместо `movementLocks()`.
