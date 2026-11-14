# Модули

Интерфейс: `ModuleInterface` · база: `BaseModule` · менеджер: `ModuleManager`.

```php
$mod = MiniGameApi::main()?->getModuleManager()->getModule('economy');
```

Выключатели: `config.yml` → `modules.enabled.<id>`.

Полные сигнатуры менеджеров — [Справочник](./reference).

## Каталог

| ID | Назначение | Точка входа |
|----|------------|-------------|
| `datastorage` | UniversalDataStorage | `MiniGameApi::storage()` |
| `tools` | Утилиты `Tools` | `Tools::…` |
| `groups` | Группы / привилегии | `getModule('groups')->getManager()` |
| `chat` | Чат, неймтеги, титулы | `getModule('chat')` |
| `auth` | Аутентификация | `AuthModule::getAuthManager()` |
| `individualids` | Уникальные ID | модуль |
| `vkinteract` | VK-уведомления | `getManager()` |
| `timeouts` | Временные ограничения | timeouts Manager |
| `punish` | Ban / mute / kick | punish Manager |
| `economy` | Деньги | `MiniGameApi::economy()` |
| `scoreboard` | Скорборды | через `hud()` |
| `bossbar` | Боссбары | через `hud()` |
| `floatingtext` | Плавающий текст | модуль |
| `npc` | NPC | `/npc` |
| `cosmetics` | Косметика | `/cosmetics` |
| `npcai` | AI + каталог | `MiniGameApi::npcAi()` |
| `teamgame` | Командный режим | `MiniGameApi::teamGame()` |

---

## Economy — `economy()`

```php
$eco = MiniGameApi::economy();
```

Типичные операции (см. Manager в справочнике): баланс, `addMoney` / `reduceMoney` / `setMoney`, переводы. Команды: `/money`, `/pay`, `/eco`.

Права: `economy.money`, `economy.pay`, `economy.admin`.

## Auth

`AuthModule::getAuthManager()`:

| Метод | Смысл |
|-------|--------|
| `needForAuth` / `getAuthStatus` / `setAuthStatus` | Состояние |
| `registerPlayer` / `validPassword` / `setForAuth` | Регистрация / логин |

Право: `auth.admin`.

## BossBar / ScoreBoard

Предпочтительно через фасад:

```php
MiniGameApi::hud()?->showBossBar($player, 'Title', 0.75);
MiniGameApi::hud()?->showScoreboard($player, 'lobby');
MiniGameApi::hud()?->clearAll($player);
```

Низкоуровнево: `BossBarModule::getManager()` — `showBossBar`, `updateBossBar`, `showTimer`, `showProgressBar`, `removeBossBar`.

## Punish / Timeouts

Punish-команды: `/ban`, `/unban`, `/mute`, `/unmute`, `/kick`, `/check`.

Timeouts Manager: `banPlayer`, `mutePlayer`, `kickPlayer`, `unbanPlayer`, `unmutePlayer`, `isBanned`, `isMuted`, `getPlayerHistory`.

Права: `punish.*`.

## NPC / NpcAi

- Классический NPC: `/npc`, право `minigamebase.npc`
- AI: `MiniGameApi::npcAi()`

```php
$ai = MiniGameApi::npcAi();
$ai?->catalog()->registerCustom(...);
$ai?->spawnCatalog($world, $pos, 'zombie_guard', ['role' => 'hostile']);
$ai?->spawnAt($world, $pos, $settings);
$ai?->despawnNear($world, $pos, 12.0);
```

| Метод | Описание |
|-------|----------|
| `catalog()` / `brain()` / `spawner()` | Подсистемы |
| `spawnAt` / `spawnCatalog` | Спавн |
| `despawn` / `despawnNear` | Удаление |

Roles: `idle`, `wander`, `hostile`, `guard`, `flee`. Право: `minigamebase.npcai`. Команда: `/npcai spawn|list|despawnnear`.

## TeamGame — `teamGame()`

```php
$tg = MiniGameApi::teamGame();
$tg?->startMatch($arena);
$tg?->sessionOf($arena);
$tg?->openItemShop($arena, $player);
$tg?->tickSecond($arena);
```

| Метод | Описание |
|-------|----------|
| `cfg(?Arena)` | Runtime knobs |
| `shop()` | ShopService |
| `sessionOf` / `sessions` | Сессии |
| `startMatch` / `onGameEnd` / `scheduleGameEnd` | Lifecycle |
| `selectLobbyTeam` / `giveWaitingLobbyItems` | Лобби |
| `onPlayerDeath` | Смерть |
| `openItemShop` / `openUpgradeShop` | Магазины |
| `broadcast` / `lang` | Сообщения |

Конфиг ядра: `teamgame.mode`, `fallback_radius`, `duration_seconds`.  
Каталог / generators / spawn_kit — в конфиге **GameType**.

События: `TeamObjectiveBreakEvent`, `TeamObjectiveDestroyEvent`.

## Groups / Chat / Cosmetics / VK / FloatingText / IndividualIDS

Серверные модули. Получайте через `ModuleManager::getModule($id)` и используйте их Manager (см. [справочник](./reference)). Не делайте жёсткую зависимость в GameType без проверки `isModuleLoaded`.

## ModuleInterface

| Метод | Роль |
|-------|------|
| `initialize(Main): bool` | Старт |
| `getName` / `getVersion` / `getDescription` | Мета |
| `getDependencies(): array` | Зависимости |
| `isCompatible(string $fwVersion): bool` | |
| `shutdown(): void` | Стоп |
| `getLoadPriority(): int` | Порядок |
| `handleFrameworkEvent(string, array): void` | Хуки |
