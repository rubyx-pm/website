---
title: MiniGameBase 2.1 — интеграции
outline: deep
description: "Фасады MiniGameApi, события, новые узлы Blueprint."
---

# Integrations — MiniGameBase

Каталог узлов и схема API: `integrations/MiniGameBase/`.

| Файл / папка | Назначение |
|--------------|------------|
| `manifest.json` | id интеграции, версия каталога, `mgbVersion` |
| `schema/api-schema.json` | Поверхности MiniGameApi / GameType / Arena / events |
| `nodes/*.json` | Определения узлов редактора → `GET /api/nodes` |
| `events/*.json` | Метаданные событий (source, cancellable) |
| `generators/registry.json` | Карта `typeId` → emitter для компилятора |
| `types/` | (опц.) общие типы пинов |

При изменении API MiniGameBase: обновить schema + nodes + registry, bump `manifest` / `api-schema.mgbVersion`, пересобрать шаблоны.

Текущая поверхность: **mgbVersion `2.1.0`** (см. `schema/api-schema.json`).

---

## MiniGameApi 2.1 — новые фасады

| Фасад | Назначение |
|-------|------------|
| `matchSpectators()` | Spectator **внутри матча** (мир арены). Не путать с `spectators()` (хаб). |
| `hud()` | Scoreboard / bossbar / actionbar / tip / `clearAll` |
| `movementLocks()` | Immobile + якорь + TTL (тики сервера) |
| `zones()` | Shrink / `setFixed` / `isInside` / урон вне радиуса (`tickSecond`) |
| `layouts()` | Именованные точки карты (`layout` в `default_maps`) + `teleport` |

### События PHP

| Класс | Когда |
|-------|--------|
| `ArenaPhaseChangeEvent` | Смена статуса waiting→voting→lobby→playing→finished |
| `MatchSecondEvent` | Раз в секунду при `playing` (`Arena::tickSecond`) |
| `PlayerEnterMatchSpectatorEvent` | Вход в match-spectator (cancellable) |
| `PlayerLeaveMatchSpectatorEvent` | Выход из match-spectator |

### Хуки GameType

| Метод | Когда |
|-------|--------|
| `onPhaseChange($arena, $from, $to)` | После смены статуса арены |
| `onMatchSecond($arena, $elapsedSeconds)` | Секундный тик матча |
| `onMatchSpectatorEnter($arena, $player, $reason)` | После успешного `matchSpectators()->enter` |

Компилятор эмитит тела для `onMatchSecond` / `onPhaseChange` из графов с entry `gt.onMatchSecond` / `gt.onPhaseChange` (и ArchitectureGenerator → MatchFlow).

---

## Новые / обновлённые узлы Blueprint

### Spectator

| id | PHP |
|----|-----|
| `spectator.enter` / `spectator.leave` | Хаб-спектатор (`MiniGameApi::spectators()`) |
| `spectator.matchEnter` | `matchSpectators()->enter($player, $arena, $reason)` |
| `spectator.matchLeave` | `matchSpectators()->leave($player)` |

Для смерти в матче используйте **`spectator.matchEnter`**, не `spectator.enter`.

### HUD

| id | PHP |
|----|-----|
| `hud.scoreboard` | `hud()->showScoreboard($player, $template)` |
| `hud.actionbar` | `hud()->actionBar($player, $text)` |
| `hud.clear` | `hud()->clearAll($player)` |
| `scoreboard.show` | Legacy через модуль scoreboard (совместимость) |

### Movement / Zone / Layout

| id | PHP |
|----|-----|
| `player.lockMovement` | `movementLocks()->lock(..., $ttlTicks)` |
| `player.unlockMovement` | `movementLocks()->unlock($player)` |
| `zone.shrink` | `zones()->startShrink(...)` |
| `zone.isInside` | Проверка точки (flow Inside/Outside) |
| `arena.teleportTo` | `layouts()->teleport($player, $arena, $pointId)` |

### Events / GameType hooks

| id | Роль |
|----|------|
| `gt.onMatchSecond` | Entry → `GameType::onMatchSecond` |
| `gt.onPhaseChange` | Entry → `GameType::onPhaseChange` |
| `ev.ArenaPhaseChange` | `ArenaPhaseChangeEvent` |
| `ev.MatchSecond` | `MatchSecondEvent` |
| `ev.PlayerEnterMatchSpectator` | `PlayerEnterMatchSpectatorEvent` |

---

## Layout в конфиге карты

`Settings::$DEFAULT_MAPS[$mapId]['layout']`:

```yaml
layout:
  spawn: [x, y, z]
  exit: { x: 0, y: 64, z: 0, yaw: 90 }
  gen.1: { pos: [10, 70, -5], yaw: 0 }
```

Узел `arena.teleportTo` / `layouts()->get|teleport`.

---

## Zone phases (игры)

Непрерывный shrink: узел `zone.shrink` или `zones()->startShrink`.

Дискретные фазы (DinoPark evacuation): игра вызывает `zones()->setFixed($id, $arena, $center, $radius, $damage)` по конфигу `dinopark.zone.phases`; урон наносит `ZoneService::tickSecond` (Main, 1 Hz). Tips / aggro остаются в игровом коде.

---

## Анти-паттерны (schema)

- `registerGameType` вне `MiniGameBaseReadyEvent` / `mg.ready`
- Удаление template-миров
- `PlayerMove` без throttle (для lock используйте `movementLocks` + listener)

Рантайм: плагин MiniGameBase на сервере (`docs/API.md`). Каталог Blueprint: репозиторий [rubyx-blueprint](https://github.com/norachcharon/rubyx-blueprint).
