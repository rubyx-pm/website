# GameType

Абстрактный класс: `MiniGameBase\types\GameType`.

Режим — отдельный плагин с `depend: [MiniGameBase]`. Ядро вызывает хуки; режим **не** дублирует lifecycle мира/статусов арены.

## Обязательные методы

| Метод | Когда |
|-------|-------|
| `onGameStart(Arena $arena)` | Матч стартовал, мир копии готов |
| `onGameEnd(Arena $arena, ?Player $winner)` | Матч завершён |
| `onPlayerJoin(Arena $arena, Player $player)` | Игрок добавлен в арену |
| `onPlayerLeave(Arena $arena, Player $player)` | Игрок вышел |
| `onPlayerDeath(Arena, Player, ?Player $killer, string $deathCause)` | Смерть в арене |
| `preparePlayer(Arena $arena, Player $player)` | Инвентарь / состояние к матчу |

## Опциональные хуки

| Метод | Назначение |
|-------|------------|
| `initialize()` | Инициализация `$this->settings` в конструкторе |
| `onVoteStart(Arena $arena)` | Старт голосования |
| `onLobbyStart(Arena $arena)` | Countdown-лобби после vote |
| `onGameTimeLimitReached(Arena $arena)` | Истёк таймер раунда (до `stopGame`) |
| `onPlayerDisconnectedDuringGame(Arena, string $playerName)` | Дисконнект со слотом reconnect |
| `onPhaseChange(Arena, string $from, string $to)` | **2.1** смена фазы |
| `onMatchSecond(Arena, int $elapsedSeconds)` | **2.1** секундный тик `playing` |
| `onMatchSpectatorEnter(Arena, Player, string $reason)` | **2.1** in-match spectator |
| `getRecommendedArenaSettings(): array` | Рекомендуемые флаги |
| `validateArenaSettings(array $settings): bool` | Валидация |
| `getGameStartMessage()` / `getWinMessage(Player)` | Тексты |
| `getGameInfoActionBarAliveCount(Arena): ?int` | Число «живых» для ActionBar; `null` = все |
| `shouldSendGameInfoActionBar(Arena): bool` | Отключить стандартный ActionBar MGB |
| `getSettings()` / `getSetting` / `setSetting` / `hasSetting` | Локальные настройки типа |

Фазы `$from` / `$to`: `waiting` | `voting` | `lobby` | `playing` | `finished`.

## Регистрация

```php
MiniGameApi::registerGameType('dbd', \DbDGame\DbDGame::class);
$gt = MiniGameApi::gameTypes()?->getGameType('dbd');
```

### GameTypeManager

| Метод | Описание |
|-------|----------|
| `registerGameType(string $id, string $class): bool` | Класс существует и extends `GameType` |
| `getGameType(string $id): ?GameType` | Экземпляр |
| `getAllGameTypes(): array` | id → class |
| `getAllGameTypeInstances(): array` | id → instance |
| `isRegistered` / `unregisterGameType` / `clearAllGameTypes` | |
| `getGameTypesInfo(): array` | Сводка для отладки |

`id` типа = `game_type` в `arena_templates`.

## Рекомендуемые настройки арены

Ключи из `getRecommendedArenaSettings()`:

| Ключ | Смысл |
|------|--------|
| `min_players` / `max_players` | Лимиты |
| `pvp_enabled` | PvP |
| `damage_enabled` | Урон |
| `hunger_enabled` | Голод |
| `fall_damage` | Урон от падения |
| `block_break` / `block_place` | Ломать / ставить блоки |

## Пример тика и spectator

```php
public function onMatchSecond(Arena $arena, int $elapsedSeconds): void
{
    MiniGameApi::zones()?->tickSecond($arena);
}

public function onPlayerDeath(Arena $arena, Player $player, ?Player $killer = null, string $deathCause = 'unknown'): void
{
    MiniGameApi::matchSpectators()?->enter($player, $arena, 'died');
}
```
