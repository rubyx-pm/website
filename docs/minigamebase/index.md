# MiniGameBase

**Версия:** 2.1 · **API PocketMine:** 5.0.0 · **PHP:** ^8.1

Фреймворк мини-игр для **PocketMine-MP 5** / **rubyx-server**: арены, лобби, голосование за карту, матчмейкинг, HUD, зоны, movement-lock, in-match spectator, модули (economy, punish, npcai, teamgame…) и фасад `MiniGameApi` для режимов вроде **DinoPark** / **DbDGame**.

## Документация

| Раздел | Содержание |
|--------|------------|
| [Быстрый старт](./guide) | Зависимость, `GameType`, ReadyEvent, шаблон арены |
| [Фасад MiniGameApi](./api) | Все точки входа фасада |
| [GameType](./gametype) | Хуки режима и настройки |
| [Арены](./arena) | Lifecycle, Arena / ArenaManager |
| [Сервисы 2.1](./services) | HUD, zones, locks, spectators, party, matchmaking… |
| [События](./events) | Все `MiniGameBase\event\*` |
| [Модули](./modules) | Встроенные модули и выключатели |
| [Команды и права](./commands) | `/mg`, `/npcai`, permissions |
| [Конфиг](./config) | `config.yml`, шаблоны, layout точек |
| [Storage / Forms / Worlds](./storage) | Данные, UI, копии карт, PlaceholderAPI |

Связанное: [RUBYX Blueprint](/docs/blueprint/) · [интеграция Blueprint ↔ MiniGameBase 2.1](/docs/blueprint/integrations)

## Архитектура

```
Loader (PluginBase)
  └─ Main — менеджеры + ModuleManager + сервисы 2.1
       ├─ ArenaManager / WorldManager / LobbyManager
       ├─ GameTypeManager / TimerManager / FormManager
       ├─ StatisticsManager / CommandManager / UniversalDataStorage
       ├─ Matchmaking · Party · HUD · Zones · MovementLock · Spectators · Layouts
       └─ MiniGameApi — статический фасад для внешних плагинов
```

## Минимальный вход

```php
use MiniGameBase\api\MiniGameApi;
use MiniGameBase\event\MiniGameBaseReadyEvent;

public function onReady(MiniGameBaseReadyEvent $event): void
{
    MiniGameApi::registerGameType('mymode', MyGameType::class);
}
```

До `MiniGameBaseReadyEvent` методы фасада могут вернуть `null`.
