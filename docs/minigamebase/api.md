# Фасад MiniGameApi

Пространство имён: `MiniGameBase\api\MiniGameApi` (final, static).

Все методы менеджеров/сервисов возвращают `null`, пока ядро не готово — проверяйте после `MiniGameBaseReadyEvent`.

## Ядро и менеджеры

| Метод | Возврат | Описание |
|-------|---------|----------|
| `main()` | `?Main` | Оркестратор |
| `loader()` | `?Loader` | PluginBase |
| `arenaManager()` | `?ArenaManager` | Арены |
| `gameTypes()` | `?GameTypeManager` | Типы игр |
| `storage()` | `?IDataStorage` | Данные / статистика |
| `timers()` | `?TimerManager` | Таймеры арен |
| `forms()` | `?FormManager` | Отправка форм |
| `commands()` | `?CommandManager` | PluginOwned-команды режимов |

Через `Main` (если нужен объект вне фасада):

```php
$main = MiniGameApi::main();
$main?->getWorldManager();
$main?->getLobbyManager();
$main?->getStatisticsManager();
$main?->getModuleManager();
$main?->getHubStatusService();
$main?->getDoctorService();
$main?->getSeasonService();
$main?->getAchievementService();
$main?->getStatsExporter();
$main?->getArenaPoolScheduler();
$main?->getSystemInfo();
$main?->getVersion();
```

## Сервисы 2.1

| Метод | Сервис | Назначение |
|-------|--------|------------|
| `matchmaking()` | `MatchmakingService` | Очередь по game type |
| `parties()` | `PartyManager` | Партии |
| `spectators()` | `SpectatorService` | Hub-spectator (выходит из арены) |
| `matchSpectators()` | `MatchSpectatorService` | In-match spectator (остаётся в мире матча) |
| `hud()` | `HudService` | Scoreboard / bossbar / actionbar / tip |
| `movementLocks()` | `MovementLockService` | Immobile + якорь + TTL |
| `zones()` | `ZoneService` | Shrink / fixed damage zones |
| `layouts()` | `LayoutCatalog` | Именованные точки карты |
| `menuActions()` | `MenuActionRegistry` | Кнопки главного меню |

Подробности методов — [Сервисы](./services).

## Модули через фасад

| Метод | Модуль | Описание |
|-------|--------|----------|
| `teamGame()` | `teamgame` | `TeamGameService` (BedWars-like) |
| `economy()` | `economy` | `EconomyManager` |
| `npcAi()` | `npcai` | Каталог + AI-спавн NPC |

Остальные модули: `MiniGameApi::main()?->getModuleManager()->getModule('auth')` и т.д. — [Модули](./modules).

## Регистрация и UI

| Метод | Описание |
|-------|----------|
| `registerGameType(string $id, string $gameTypeClass): bool` | Класс должен наследовать `GameType` |
| `registerMenuAction(string $id, string $label, callable $handler, ?string $permission = null): void` | Кнопка в MainMenu |

## Scheduler и сообщения

| Метод | Описание |
|-------|----------|
| `scheduleDelayed(Closure $task, int $delayTicks = 1): ?TaskHandler` | Отложенная задача |
| `scheduleRepeating(Closure $task, int $periodTicks): ?TaskHandler` | Повтор; отмена через `$h->cancel()` |
| `sendLang(Player $player, string $tfColor, string $langKey, array $params = []): void` | Префикс + Lang |

## Права

| Метод | Описание |
|-------|----------|
| `hasPermission(CommandSender, string): bool` | |
| `requirePermission(CommandSender, string, ?string $deny = null): bool` | |
| `requireAnyPermission(CommandSender, list\<string\>, ?string $deny = null): bool` | |
| `requirePlayer(CommandSender, ?string $deny = null): ?Player` | |

Обёртки над `MiniGameBase\utils\Perms`.

## Свои команды режима

```php
MiniGameApi::commands()?->register(
    $this->plugin,
    'mygame',
    'My mini-game',
    '/mygame [help|debug]',
    ['mgame'],
    'mygame.use',
    function ($sender, string $label, array $args): bool {
        return true;
    },
    playerOnly: true
);
```

Права объявляйте в своём `plugin.yml` (`permissions:`), **без** секции `commands:` — иначе PluginCommand перехватит вызов.
