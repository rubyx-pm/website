# Команды и права

`/minigame` и `/mg` регистрируются в коде (`MiniGameCommand`), **не** в `plugin.yml`.

## /mg

| Команда | Право | Описание |
|---------|-------|----------|
| `/mg join [id\|gameType]` | `minigame.join` | Join / матчмейкинг |
| `/mg leave` | `minigame.leave` | Выйти (+ dequeue) |
| `/mg queue` / `/mg leavequeue` | `minigame.leave` | Выйти из очереди |
| `/mg vote <номер>` | `minigame.vote` | Голос за карту |
| `/mg stats` | `minigame.stats` | Статистика |
| `/mg list [страница]` | `minigame.info` | Список арен |
| `/mg info` | `minigame.info` | Текущая арена |
| `/mg top [wins\|kills\|games]` | `minigame.top` | Топ |
| `/mg biome` | `minigame.biome` | Биом |
| `/mg party create\|invite\|accept\|leave\|info` | `minigame.use` | Партия |
| `/mg spectate [arenaId]\|leave` | `minigame.use` | Hub-spectator |
| `/mg arena list\|create\|delete` | `minigame.admin` | Управление аренами |
| `/mg reload` | `minigame.admin` | Reload конфига |
| `/mg debugstart <id> [map]` | `minigame.admin` | Тестовый старт |
| `/mg doctor` | `minigame.admin` | Диагностика миров/типов |
| `/mg season info\|start <id>` | `minigame.admin` | Сезоны статистики |
| `/mg help` | `minigame.use` | Справка |

## Модули

| Модуль | Команды | Права |
|--------|---------|--------|
| Auth | меню аккаунта, admin-reg | `auth.admin` |
| Economy | `/money`, `/pay`, `/eco` | `economy.money`, `economy.pay`, `economy.admin` |
| Punish | `/ban`, `/unban`, `/mute`, `/unmute`, `/kick`, `/check` | `punish.*` |
| NPC | `/npc` | `minigamebase.npc` |
| NpcAi | `/npcai spawn\|list\|despawnnear` | `minigamebase.npcai` |
| Cosmetics | `/cosmetics` | `minigame.cosmetics` |
| Groups | `/group` | Groups + `groups.super_access` |

## Permissions (plugin.yml)

| Право | Default | Назначение |
|-------|---------|------------|
| `minigame.use` | true | Базовый `/mg` |
| `minigame.join` / `leave` / `stats` / `top` / `info` / `vote` / `biome` | true | Подкоманды |
| `minigame.admin` | op | arena, reload, debugstart, doctor, season |
| `auth.admin` | op | Auth admin |
| `economy.money` / `pay` | true | Баланс / переводы |
| `economy.admin` | op | `/eco` |
| `minigamebase.npc` | op | `/npc` |
| `minigamebase.npcai` | op | `/npcai` |
| `minigame.cosmetics` | true | `/cosmetics` |
| `punish.ban` / `unban` / `mute` / `unmute` / `kick` / `check` | op | Наказания |

## Свои команды режима

Через `MiniGameApi::commands()` в `MiniGameBaseReadyEvent`. Права — в своём `plugin.yml`, без секции `commands:`.

```php
use MiniGameBase\utils\Perms;

Perms::require($sender, 'mygame.debug');
Perms::requirePlayer($sender);
```
