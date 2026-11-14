# Конфигурация

Файл: `plugin_data/MiniGameBase/config.yml` (шаблон — `resources/config.yml`).

## Ключевые секции

| Секция / ключ | Описание |
|---------------|----------|
| `database` | MySQL DSN, user/password; `shutdown_server_on_failure` |
| `language` | `ru_RU` / `en_US` |
| `game.game_time` / `lobby_time` / `vote_time` | Длительности (сек) |
| `game.reconnect_enabled` / `reconnect_timeout` | Reconnect |
| `worlds.main_lobby` / `game_world` | Хаб / дефолт игры |
| `world_time.stop_on_load` | `stopTime` на загрузке |
| `arena_templates` | Шаблоны арен |
| `default_maps` | Карты + `layout` |
| `arena_pool` | Предсоздание + schedule |
| `forms.cooldown_seconds` | Cooldown форм |
| `performance.*` | Maintenance / ActionBar / GC |
| `commands.list_per_page` / `top_max_limit` | Лимиты `/mg list` `/mg top` |
| `groups.super_access` | Ники с полным `/group` |
| `modules.profile` / `modules.enabled.*` | Выключатели модулей |
| `teamgame.*` | Runtime knobs teamgame |
| `matchmaking.max_queue_seconds` / `fill_weight` | Очередь |
| `hub_status.enabled` / `scoreboard` | Хаб-статус |
| `stats.season_id` | Текущий сезон |
| `achievements.rules` | Правила ачивок |
| `export.webhook_url` / `interval_seconds` | Stats export |

Персист арен: `plugins/MiniGameBase/arenas.yml` (без списка игроков).

## Шаблон арены

```yaml
arena_templates:
  mymode:
    name_template: "Мой режим {number}"
    available_maps: ["map_1"]
    min_players: 2
    max_players: 8
    game_type: "mymode"          # = registerGameType id
    default_world_time: 6000     # опционально
```

## Карты и layout

```yaml
default_maps:
  map_1:
    name: "Первая карта"
    world: "map_1"               # папка в worlds/
    description: "…"
    layout:
      spawn: [0, 64, 0]
      center: {x: 0, y: 70, z: 0, yaw: 90, pitch: 0}
      killer_spawn: [10, 64, -5]
```

Точки читает `MiniGameApi::layouts()`.

## Пул арен

```yaml
arena_pool:
  enabled: true
  count: 2
  template: mymode
  max_arenas: 8
  schedule: []                   # см. ArenaPoolScheduler
```

## Модули

```yaml
modules:
  profile: full
  enabled:
    economy: true
    punish: true
    npcai: true
    teamgame: true
    # …
```

## Achievements / export / season

```yaml
stats:
  season_id: season_1

achievements:
  rules:
    - id: wins_10
      stat: wins
      threshold: 10
      reward_money: 100
      reward_title: Ветеран

export:
  webhook_url: ''
  interval_seconds: 0
```

Сезоны: `/mg season info|start <id>` → ключи статы через `SeasonService::statKey`.
