# Модули

Интерфейс: `MiniGameBase\modules\ModuleInterface`  
База: `BaseModule` · менеджер: `ModuleManager`

```php
$mod = MiniGameApi::main()?->getModuleManager()->getModule('economy');
```

Выключатели: `config.yml` → `modules.enabled.<id>: true|false`.  
Профиль: `modules.profile: full`.

## Каталог

| ID | Класс | Назначение | Фасад / команды |
|----|-------|------------|-----------------|
| `datastorage` | `builtin\DataStorageModule` | UniversalDataStorage | `MiniGameApi::storage()` |
| `tools` | `builtin\ToolsModule` | Утилиты `Tools` | — |
| `groups` | `groups\GroupsModule` | Группы / привилегии | `/group` |
| `chat` | `chat\ChatModule` | Чат, неймтеги, титулы | — |
| `auth` | `auth\AuthModule` | Аутентификация | `auth.admin` |
| `individualids` | `individualids\IndividualIDSModule` | Уникальные ID | — |
| `vkinteract` | `vkinteract\VKInteractModule` | VK-уведомления | — |
| `timeouts` | `timeouts\TimeoutsModule` | Временные ограничения | — |
| `punish` | `punish\PunishSystemModule` | Ban / mute / kick | `/ban` `/mute` `/kick`… |
| `economy` | `economy\EconomyModule` | Деньги | `MiniGameApi::economy()`, `/money` `/pay` `/eco` |
| `scoreboard` | `scoreboard\ScoreBoardModule` | Скорборды | через `hud()` |
| `bossbar` | `bossbar\BossBarModule` | Боссбары | через `hud()` |
| `floatingtext` | `floatingtext\FloatingTextModule` | Плавающий текст / топы | — |
| `npc` | `npc\NPCModule` | NPC | `/npc` |
| `cosmetics` | `cosmetics\CosmeticsModule` | Косметика | `/cosmetics` |
| `npcai` | `npcai\NpcAiModule` | AI + каталог моделей | `MiniGameApi::npcAi()`, `/npcai` |
| `teamgame` | `teamgame\TeamGameModule` | Командный режим (BedWars-like) | `MiniGameApi::teamGame()` |

## NpcAi

```php
$ai = MiniGameApi::npcAi();
$ai?->catalog()->registerCustom(...);
$ai?->spawnCatalog($world, $pos, 'zombie_guard', ['role' => 'hostile']);
$ai?->spawnAt($world, $pos, $settings);
$ai?->despawnNear($world, $pos, 12.0);
```

| Метод сервиса | Описание |
|---------------|----------|
| `catalog()` / `brain()` / `spawner()` | Подсистемы |
| `spawnAt(World, Vector3, NpcSpawnSettings)` | Прямой спавн |
| `spawnCatalog(World, Vector3, string $id, array $overrides = [])` | Из каталога |
| `despawn` / `despawnNear` | Удаление |

AI roles: `idle`, `wander`, `hostile`, `guard`, `flee` (5 Hz, LOS + steering).  
Право: `minigamebase.npcai`. Команда: `/npcai spawn|list|despawnnear`.

## TeamGame

```php
$tg = MiniGameApi::teamGame();
$tg?->startMatch($arena);
$session = $tg?->sessionOf($arena);
$tg?->openItemShop($arena, $player);
```

| Метод | Описание |
|-------|----------|
| `cfg(?Arena): array` | Runtime knobs |
| `shop(): ShopService` | Магазин |
| `sessionOf(Arena): ?ArenaSession` | Сессия матча |
| `startMatch` / `onGameEnd` / `tickSecond` | Lifecycle |
| `selectLobbyTeam` / `giveWaitingLobbyItems` | Лобби |
| `onPlayerDeath` / `scheduleGameEnd` | Смерть / конец |
| `openItemShop` / `openUpgradeShop` | UI магазина |
| `broadcast` / `lang` | Сообщения |

Конфиг ядра: `teamgame.mode`, `fallback_radius`, `duration_seconds`.  
Механики режима (каталог, generators, spawn_kit) — в конфиге **GameType** (например BedWars), не в MiniGameBase.

События: `TeamObjectiveBreakEvent`, `TeamObjectiveDestroyEvent`.

## ModuleInterface

| Метод | Роль |
|-------|------|
| `initialize(Main $framework): bool` | Старт |
| `getName` / `getVersion` / `getDescription` | Мета |
| `getDependencies(): array` | Зависимости модулей |
| `isCompatible(string $frameworkVersion): bool` | |
| `shutdown(): void` | Стоп |
| `getLoadPriority(): int` | Порядок загрузки |
| `handleFrameworkEvent(string $eventType, array $data = []): void` | Хуки фреймворка |

Не считайте auth/economy обязательными для матча — проверяйте наличие модуля.
