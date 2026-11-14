---
title: RUBYX Blueprint
outline: deep
description: "Визуальная IDE мини-игр Bedrock на MiniGameBase — документация раздела Blueprint."
---

# RUBYX Blueprint

Визуальный редактор графов для мини-игр **Minecraft Bedrock** на рантайме **MiniGameBase**.  
Источник истины — пакет `.rbxblueprint`; PHP-плагин — артефакт компиляции под ядро RubyxGames / PocketMine 5.

```
Vue-редактор → API → Compiler → ZIP-плагин → Build Agent → plugins/
```

## Страницы раздела

| Страница | О чём |
|----------|--------|
| [Полная документация](./guide) | UI, графы, хуки, Fn.*, сборка, API, анти-паттерны |
| [MiniGameBase 2.1](./integrations) | Фасады `matchSpectators` / `hud` / `zones` / locks / layouts |
| [Каталог узлов](./nodes) | Пины и полный список `gt.*` / `hud.*` / `zone.*` / … |
| [Шаблоны](./templates) | Empty, SkyWars, DinoPark, Bedwars и NG scaffolds |
| [Компилятор](./compiler) | Пайплайн IR → PHP, layout как DbDGame |
| [Архитектура](./architecture) | Стек monorepo, auth, async builds |
| [Формат `.rbxblueprint`](./format) | project.json, графы, resources |
| [Vs Unreal Engine](./ue-gap) | Gap-анализ: Function UX, debug, macros |

## Скриншоты {#скриншоты}

### Дашборд

![Дашборд](/img/blueprint/01-dashboard.png)

### Шаблоны

![Шаблоны](/img/blueprint/02-templates.png)

### Редактор — Spectator (DinoPark)

![Spectator](/img/blueprint/03-editor-spectator.png)

### Evacuation / zone

![Zone](/img/blueprint/04-editor-zone.png)

### HUD / Scoreboard

![Scoreboard](/img/blueprint/05-editor-scoreboard.png)

### GameType lifecycle

![GameType](/img/blueprint/06-editor-gametype.png)

## С чего начать

1. Поднять IDE (см. [быстрый старт](./guide#2-быстрый-старт)).
2. Создать проект из шаблона или **Мастера новой игры**.
3. Собрать `gt.onGameStart` → `player.sendMessage`.
4. **Проверить** → **Собрать** → при необходимости пуш на test-сервер.

## MiniGameBase <span id="minigamebase"></span>

Сгенерированные плагины зависят от `MiniGameBase` (`depend: [MiniGameBase]`).  
Регистрация `GameType` — только после `MiniGameBaseReadyEvent`.

**2.1:** in-match spectator, HUD-фасад, movement lock, zones, layout points, `onMatchSecond` / `onPhaseChange` — см. [интеграции](./integrations).

::: tip Связь с ядром
Плагины рассчитаны на [RubyxGames PocketMine-MP](/docs/rubyx/). Установка ядра: [релизы GitHub](/docs/rubyx/installation).
:::
