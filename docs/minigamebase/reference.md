# Справочник API

Полный перечень публичных методов MiniGameBase **2.1** (сгенерировано из исходников).

Точка входа для режимов: [`MiniGameApi`](./api). Обзор модулей: [`modules`](./modules).

## Ядро

### `MiniGameApi`

`src/MiniGameBase/api/MiniGameApi.php`

| Метод |
|-------|
| `main()` : `?Main` |
| `loader()` : `?Loader` |
| `arenaManager()` : `?ArenaManager` |
| `gameTypes()` : `?GameTypeManager` |
| `storage()` : `?IDataStorage` |
| `timers()` : `?TimerManager` |
| `forms()` : `?FormManager` |
| `commands()` : `?CommandManager` |
| `matchmaking()` : `?MatchmakingService` |
| `parties()` : `?PartyManager` |
| `spectators()` : `?SpectatorService` |
| `matchSpectators()` : `?MatchSpectatorService` |
| `hud()` : `?HudService` |
| `movementLocks()` : `?MovementLockService` |
| `zones()` : `?ZoneService` |
| `layouts()` : `?LayoutCatalog` |
| `menuActions()` : `?MenuActionRegistry` |
| `teamGame()` : `?TeamGameService` |
| `economy()` : `?EconomyManager` |
| `npcAi()` : `?NpcAiService` |
| `registerGameType(string $id, string $gameTypeClass)` : `bool` |
| `registerMenuAction(string $id, string $label, callable $handler, ?string $permission = null)` : `void` |
| `scheduleDelayed(\Closure $task, int $delayTicks = 1)` : `?TaskHandler` |
| `scheduleRepeating(\Closure $task, int $periodTicks)` : `?TaskHandler` |
| `sendLang(Player $player, string $tfColor, string $langKey, array $params = [])` : `void` |
| `hasPermission(CommandSender $sender, string $permission)` : `bool` |
| `requirePermission(CommandSender $sender, string $permission, ?string $denyMessage = null)` : `bool` |
| `requireAnyPermission(CommandSender $sender, array $permissions, ?string $denyMessage = null)` : `bool` |
| `requirePlayer(CommandSender $sender, ?string $denyMessage = null)` : `?Player` |

### `GameType`

`src/MiniGameBase/types/GameType.php`

Абстрактные + опциональные хуки. Обязательные: `onGameStart`, `onGameEnd`, `onPlayerJoin`, `onPlayerLeave`, `onPlayerDeath`, `preparePlayer`.

| Метод |
|-------|
| `onGameStart(Arena $arena)` : `void` |
| `onGameEnd(Arena $arena, ?Player $winner = NULL)` : `void` |
| `onPlayerDeath(Arena $arena, Player $player, ?Player $killer = NULL, string $deathCause = 'unknown')` : `void` |
| `onPlayerJoin(Arena $arena, Player $player)` : `void` |
| `onPlayerLeave(Arena $arena, Player $player)` : `void` |
| `onPlayerDisconnectedDuringGame(Arena $arena, string $playerName)` : `void` |
| `preparePlayer(Arena $arena, Player $player)` : `void` |
| `onVoteStart(Arena $arena)` : `void` |
| `onLobbyStart(Arena $arena)` : `void` |
| `onGameTimeLimitReached(Arena $arena)` : `void` |
| `onPhaseChange(Arena $arena, string $from, string $to)` : `void` |
| `onMatchSecond(Arena $arena, int $elapsedSeconds)` : `void` |
| `onMatchSpectatorEnter(Arena $arena, Player $player, string $reason)` : `void` |
| `getRecommendedArenaSettings()` : `array` |
| `validateArenaSettings(array $arenaSettings)` : `bool` |
| `getGameStartMessage()` : `string` |
| `getWinMessage(Player $winner)` : `string` |
| `getSettings()` : `array` |
| `getGameInfoActionBarAliveCount(Arena $arena)` : `?int` |
| `shouldSendGameInfoActionBar(Arena $arena)` : `bool` |


### `Perms`

`src/MiniGameBase/utils/Perms.php`

| Метод |
|-------|
| `has(CommandSender $sender, string $permission)` : `bool` |
| `hasAny(CommandSender $sender, array $permissions)` : `bool` |
| `hasAll(CommandSender $sender, array $permissions)` : `bool` |
| `require(CommandSender $sender, string $permission, ?string $denyMessage = null)` : `bool` |
| `requireAny(CommandSender $sender, array $permissions, ?string $denyMessage = null)` : `bool` |
| `requireAll(CommandSender $sender, array $permissions, ?string $denyMessage = null)` : `bool` |
| `deny(CommandSender $sender, ?string $message = null)` : `void` |
| `requirePlayer(CommandSender $sender, ?string $denyMessage = null)` : `?Player` |

### `Lang`

`src/MiniGameBase/utils/Lang.php`

| Метод |
|-------|
| `load(Plugin $plugin, string $locale = 'ru_RU')` : `void` |
| `has(string $key)` : `bool` |
| `get(string $key, array $replace = [])` : `string` |
| `setLocale(string $locale)` : `void` |
| `getLocale()` : `string` |

### `Tools`

`src/MiniGameBase/utils/Tools.php`

| Метод |
|-------|
| `teleportToMainLobby(Player $player, string $lobbyWorldName)` : `void` |
| `getWorldByName(string $worldName)` : `?World` |
| `teleportToGameWorld(Player $player, string $gameWorldName)` : `void` |
| `formatTime(int $seconds)` : `string` |
| `formatDurationHoursMinutes(int $seconds)` : `string` |
| `isValidWorldName(string $worldName)` : `bool` |
| `getRandomArrayElement(array $array)` |
| `isPlayerOnline(string $playerName)` : `bool` |
| `stringToTimestamp(string $timeString)` : `int` |
| `convertTime(int $seconds)` : `string` |
| `getHandleSymbolName(string $playerName)` : `string` |
| `saveVariableData(string $playerName, string $key, mixed $value)` : `void` |
| `getVariableData(string $playerName, string $key, mixed $default = null)` : `mixed` |
| `generatePassword(int $length = 8)` : `string` |
| `resolveEnvRecursive(string $text)` : `string` |

### `ModuleManager`

`src/MiniGameBase/modules/ModuleManager.php`

| Метод |
|-------|
| `loadAllModules()` : `void` |
| `loadModule(string $moduleId)` : `bool` |
| `broadcastEvent(string $eventType, array $data = [])` : `void` |
| `unloadModule(string $moduleId)` : `bool` |
| `shutdown()` : `void` |
| `isModuleLoaded(string $moduleId)` : `bool` |
| `getModule(string $moduleId)` : `?ModuleInterface` |
| `getLoadedModules()` : `array` |
| `getModulesInfo()` : `array` |
| `autoDiscoverModules()` : `void` |
| `registerModule(string $moduleId, ModuleInterface $module)` : `bool` |

## Менеджеры

### `Arena`

`src/MiniGameBase/manager/Arena.php`

| Метод |
|-------|
| `createArenaWithMode(string $modeId, ?WorldManager $worldManager = NULL, array $maps = [])` : `?Arena` |
| `addPlayer(Player $player)` : `bool` |
| `isPlayerInArena(Player $player)` : `bool` |
| `getName()` : `string` |
| `sendGameInfoActionBar()` : `void` |
| `broadcastSound(\pocketmine\world\sound\Sound $sound)` : `void` |
| `showVoteFormToPlayers()` : `void` |
| `sendVoteFormToPlayer(Player $player)` : `void` |
| `getTimerManager()` : `?TimerManager` |
| `startGame()` : `void` |
| `forceStartGameForTest(?string $mapId = null)` : `?string` |
| `stopGame(?Player $winner = NULL)` : `void` |
| `clearVotes()` : `void` |
| `getPlayerJoinDenialReason(Player $player)` : `?int` |
| `canPlayerJoin(Player $player)` : `bool` |
| `voteForMap(Player $player, int $mapIndex)` : `bool` |
| `handlePlayerDeath(Player $player, ?Player $killer = NULL, string $deathCause = 'unknown')` : `void` |
| `getId()` : `string` |
| `getPlayerCount()` : `int` |
| `getMaxPlayers()` : `int` |
| `getMinPlayers()` : `int` |
| `getStatus()` : `string` |
| `getCurrentMap()` : `?string` |
| `getAvailableMaps()` : `array` |
| `isGameRunning()` : `bool` |
| `isInLobby()` : `bool` |
| `getPlayers()` : `array` |
| `getDeadPlayers()` : `array` |
| `isPlayerDead(Player $player)` : `bool` |
| `getGameStartTime()` : `int` |
| `getGameMode()` : `?string` |
| `setGameMode(string $gameMode)` : `void` |
| `canJoin()` : `bool` |
| `markDisconnected(Player $player)` : `void` |
| `tryReconnect(Player $player)` : `bool` |
| `checkStopGameIfNoParticipantsRemain()` : `void` |
| `processDisconnectedTimeouts()` : `array` |
| `getDisconnectedPlayerNames()` : `array` |
| `setPendingVoidKiller(Player $victim, ?Player $killer)` : `void` |
| `getAndClearPendingVoidKiller(Player $victim)` : `?Player` |
| `getGameTime()` : `int` |
| `tick()` : `void` |
| `tickSecond()` : `void` |
| `getGameType()` : `string` |
| `getArenaGameMode()` : `?string` |
| `getGameWorld()` : `?World` |
| `getWorldManager()` : `?WorldManager` |
| `getVotes()` : `array` |
| `addVote(string $playerName, string $map)` : `void` |
| `isVoting()` : `bool` |
| `setVoting(bool $voting)` : `void` |
| `moveToLobby()` : `void` |
| `moveToWaiting()` : `void` |
| `addDeadPlayer(Player $player)` : `void` |
| `removePlayer(Player $player)` : `bool` |
| `getAlivePlayersCount()` : `int` |
| `getDeadPlayersCount()` : `int` |

### `ArenaManager`

`src/MiniGameBase/manager/ArenaManager.php`

| Метод |
|-------|
| `createNewArena(?string $templateId = null)` : `?Arena` |
| `setPersistencePath(?string $absolutePath)` : `void` |
| `savePersistentState()` : `void` |
| `loadArenasFromDisk()` : `void` |
| `getAllArenas()` : `array` |
| `addPlayerToArena(Player $player, ?string $arenaId = null)` : `bool` |
| `isPlayerInArena(Player $player)` : `bool` |
| `findAvailableArena(Player $player)` : `?Arena` |
| `getArenasByStatus(string $status)` : `array` |
| `getArena(string $arenaId)` : `?Arena` |
| `refreshArenaIndexes(string $arenaId)` : `void` |
| `removePlayerFromArena(Player $player)` : `bool` |
| `removeEmptyArena(string $arenaId)` : `bool` |
| `getPlayerArena(Player $player)` : `?Arena` |
| `tryReconnectPlayer(Player $player)` : `bool` |
| `removePlayerFromArenaByName(string $playerName)` : `void` |
| `clearArenaPlayerMappings(string $arenaId)` : `void` |
| `getArenasStats()` : `array` |
| `getOverallStats()` : `array` |
| `getActiveArenas()` : `array` |
| `getLobbyArenas()` : `array` |
| `getWaitingArenas()` : `array` |
| `getPerformanceStats()` : `array` |
| `processExpiredReconnects()` : `void` |
| `tick()` : `void` |
| `cleanupEmptyArenas()` : `int` |
| `getBestArenaForBalance()` : `?Arena` |
| `formatArenasList(array $stats)` : `array` |
| `sortArenasByStatus(array $stats)` : `array` |
| `getStatusText(string $status)` : `string` |
| `createPlayersBar(int $current, int $max)` : `string` |
| `formatArenaInfo($playerArena)` : `array` |
| `formatOverallStats(array $overallStats)` : `array` |
| `findArenaByGameType(string $gameType)` : `?Arena` |
| `clearAll()` : `void` |
| `stopAllArenas()` : `void` |

### `CommandManager`

`src/MiniGameBase/manager/CommandManager.php`

| Метод |
|-------|
| `register(Plugin $owner, string $name, string $description, string $usage, array $aliases, string|array|null $permissions, \Closure $handler, bool $playerOnly = false, ?string $playerOnlyMessage = null,)` : `bool` |
| `registerCommand(Plugin $owner, Command $command)` : `bool` |
| `unregister(string $name)` : `bool` |
| `isRegistered(string $name)` : `bool` |
| `get(string $name)` : `?Command` |
| `cleanupCache()` : `void` |
| `getCacheSize()` : `int` |

### `FormManager`

`src/MiniGameBase/manager/FormManager.php`

| Метод |
|-------|
| `menuActions()` : `MenuActionRegistry` |
| `registerMenuAction(string $id, string $label, callable $handler, ?string $permission = null)` : `void` |
| `sendForm(Player $player, Form $form, bool $forceOpen = false)` : `bool` |
| `sendDduiForm(Player $player, DduiForm $form, bool $forceOpen = false)` : `bool` |
| `hasOpenForm(string $playerName)` : `bool` |
| `closeForm(string $playerName)` : `void` |
| `createFormTitle(string $title)` : `string` |
| `tick()` : `void` |
| `getStats()` : `array` |
| `getStatusText(string $status)` : `string` |

### `GameTypeManager`

`src/MiniGameBase/manager/GameTypeManager.php`

| Метод |
|-------|
| `registerGameType(string $id, string $gameTypeClass)` : `bool` |
| `getAllGameTypes()` : `array` |
| `getAllGameTypeInstances()` : `array` |
| `getGameType(string $id)` : `?GameType` |
| `isRegistered(string $id)` : `bool` |
| `unregisterGameType(string $id)` : `bool` |
| `getGameTypesInfo()` : `array` |
| `clearAllGameTypes()` : `void` |

### `IDataStorage`

`src/MiniGameBase/manager/IDataStorage.php`

| Метод |
|-------|
| `getPlayerData(Player $player, string $key, mixed $default = null)` : `mixed;` |
| `setPlayerData(Player $player, string $key, mixed $value)` : `void;` |
| `setPlayerDataByName(string $playerName, string $key, mixed $value)` : `void;` |
| `incrementPlayerData(Player $player, string $key, int $amount = 1)` : `void;` |
| `getAllPlayerData(Player $player)` : `array;` |
| `getPlayerDataByName(string $playerName, string $key, mixed $default = null)` : `mixed;` |
| `getTopPlayers(string $key, int $limit = 10)` : `array;` |
| `deletePlayerData(Player $player, string $key)` : `void;` |
| `clearPlayerData(Player $player)` : `void;` |
| `getPlayersCount(string $key)` : `int;` |
| `getKeyStatistics(string $key)` : `array;` |
| `clearPlayerCache(string $playerName)` : `void;` |
| `clearAllCache()` : `void;` |
| `getPlayerDataKeys(Player $player)` : `array;` |
| `hasPlayerData(Player $player, string $key)` : `bool;` |
| `saveAll()` : `void;` |
| `getPlayerStats(Player $player)` : `array;` |
| `updatePlayerStats(Player $player, string $statType, int $value = 1)` : `void;` |
| `setPlayerStats(Player $player, string $statType, int $value)` : `void;` |

### `LobbyManager`

`src/MiniGameBase/manager/LobbyManager.php`

| Метод |
|-------|
| `teleportToLobby(Player $player)` : `void` |
| `prepareLobbyPlayer(Player $player)` : `void` |

### `MySQLBaseManager`

`src/MiniGameBase/manager/MySQLBaseManager.php`

| Метод |
|-------|
| `getInstance()` : `MySQLBaseManager` |
| `run(PrepareObject $object)` : `array|bool` |
| `getPDO()` : `?PDO` |
| `getConnection()` : `?PDO` |
| `isConnected()` : `bool` |

### `StatisticsManager`

`src/MiniGameBase/manager/StatisticsManager.php`

| Метод |
|-------|
| `updatePlayerStat(Player $player, string $statType, int $value)` : `void` |
| `getPlayerStats(Player $player)` : `array` |
| `getTopPlayers(string $statType, int $limit = 10)` : `array` |
| `saveAll()` : `void` |

### `TimerManager`

`src/MiniGameBase/manager/TimerManager.php`

| Метод |
|-------|
| `startLobbyTimer(string $arenaId, int $duration, callable $callback)` : `void` |
| `startGameTimer(string $arenaId, int $duration, callable $callback)` : `void` |
| `stopArenaTimers(string $arenaId)` : `void` |
| `stopTimer(string $timerId)` : `void` |
| `clearAllTimers()` : `void` |
| `getActiveTimersCount()` : `int` |
| `startVoteTimer(string $arenaId, int $duration, callable $callback)` : `void` |
| `startRepeatingTimer(string $timerId, int $period, callable $callback)` : `void` |
| `isTimerActive(string $timerId)` : `bool` |
| `getRemainingTime(string $timerId)` : `int` |
| `stopAllTimers(string $arenaId)` : `void` |

### `UniversalDataStorage`

`src/MiniGameBase/manager/UniversalDataStorage.php`

| Метод |
|-------|
| `getPlayerDataByName(string $playerName, string $key, mixed $default = null)` : `mixed` |
| `getTopPlayers(string $key, int $limit = 10)` : `array` |
| `deletePlayerData(Player $player, string $key)` : `void` |
| `deletePlayerDataByName(string $playerName, string $key)` : `void` |
| `clearPlayerData(Player $player)` : `void` |
| `clearPlayerCache(string $playerName)` : `void` |
| `getPlayersCount(string $key)` : `int` |
| `getKeyStatistics(string $key)` : `array` |
| `clearAllCache()` : `void` |
| `getPlayerDataKeys(Player $player)` : `array` |
| `hasPlayerData(Player $player, string $key)` : `bool` |
| `getPlayerData(Player $player, string $key, mixed $default = null)` : `mixed` |
| `saveAll()` : `void` |
| `getPlayerStats(Player $player)` : `array` |
| `getAllPlayerData(Player $player)` : `array` |
| `updatePlayerStats(Player $player, string $statType, int $value = 1)` : `void` |
| `incrementPlayerData(Player $player, string $key, int $amount = 1)` : `void` |
| `setPlayerData(Player $player, string $key, mixed $value)` : `void` |
| `setPlayerDataByName(string $playerName, string $key, mixed $value)` : `void` |
| `setPlayerStats(Player $player, string $statType, int $value)` : `void` |

### `WorldManager`

`src/MiniGameBase/manager/WorldManager.php`

| Метод |
|-------|
| `createGameInstance(string $templateWorld, string $instanceName)` : `?World` |
| `getWorldByName(string $worldName)` : `?World` |
| `getGameInstance(string $instanceName)` : `?World` |
| `getAllGameInstances()` : `array` |
| `clearAllGameInstances()` : `void` |
| `unloadGameInstance(string $instanceName)` : `bool` |
| `removeGameInstance(string $instanceName)` : `bool` |

## Сервисы 2.1

### `AchievementService`

`src/MiniGameBase/service/AchievementService.php`

| Метод |
|-------|
| `evaluatePlayer(Player $player)` : `void` |

### `ArenaJoinService`

`src/MiniGameBase/service/ArenaJoinService.php`

| Метод |
|-------|
| `tryJoin(Player $player, ?string $arenaId = null)` : `void` |

### `ArenaPoolScheduler`

`src/MiniGameBase/service/ArenaPoolScheduler.php`

| Метод |
|-------|
| `tick()` : `void` |

### `DoctorService`

`src/MiniGameBase/service/DoctorService.php`

| Метод |
|-------|
| `run(CommandSender $sender)` : `void` |

### `HubStatusService`

`src/MiniGameBase/service/HubStatusService.php`

| Метод |
|-------|
| `tick()` : `void` |

### `HudService`

`src/MiniGameBase/service/HudService.php`

| Метод |
|-------|
| `showScoreboard(Player $player, string $template)` : `bool` |
| `clearScoreboard(Player $player)` : `void` |
| `showBossBar(Player $player, string $title, float $progress = 1.0, int $color = 5)` : `?int` |
| `updateBossBar(Player $player, ?string $title = null, ?float $progress = null, ?int $color = null)` : `bool` |
| `clearBossBar(Player $player)` : `void` |
| `actionBar(Player $player, string $text)` : `void` |
| `tip(Player $player, string $text)` : `void` |
| `clearAll(Player $player)` : `void` |

### `LayoutCatalog`

`src/MiniGameBase/service/LayoutCatalog.php`

| Метод |
|-------|
| `get(Arena $arena, string $pointId)` : `?Location` |
| `all(Arena $arena)` : `array` |
| `teleport(Player $player, Arena $arena, string $pointId)` : `bool` |
| `has(Arena $arena, string $pointId)` : `bool` |

### `MatchSpectatorService`

`src/MiniGameBase/service/MatchSpectatorService.php`

| Метод |
|-------|
| `enter(Player $player, Arena $arena, string $reason = 'died', bool $hideFromPlayers = true)` : `bool` |
| `leave(Player $player, string $reason = 'leave')` : `bool` |
| `isSpectator(string $playerName)` : `bool` |
| `getArenaId(string $playerName)` : `?string` |
| `getRespawnPosition(Player $player)` : `?Position` |
| `listArenaSpectatorNames(string $arenaId)` : `array` |
| `clearArena(string $arenaId)` : `void` |
| `onQuit(Player $player)` : `void` |

### `MatchmakingService`

`src/MiniGameBase/service/MatchmakingService.php`

| Метод |
|-------|
| `enqueue(Player $player, string $gameType = '')` : `bool` |
| `dequeue(Player $player, string $reason = 'leave')` : `void` |
| `isQueued(string $playerName)` : `bool` |
| `tick()` : `void` |

### `MovementLockService`

`src/MiniGameBase/service/MovementLockService.php`

| Метод |
|-------|
| `lock(Player $player, ?Vector3 $anchor = null, ?string $arenaId = null, ?int $ttlTicks = null, bool $immobile = true)` : `void` |
| `unlock(Player $player)` : `void` |
| `isLocked(string $playerName)` : `bool` |
| `getAnchor(string $playerName)` : `?Position` |
| `tick()` : `void` |
| `enforce(Player $player)` : `void` |
| `clearArena(?string $arenaId)` : `void` |
| `onQuit(Player $player)` : `void` |

### `PartyManager`

`src/MiniGameBase/service/PartyManager.php`

| Метод |
|-------|
| `create(Player $leader)` : `bool` |
| `invite(Player $leader, string $targetName)` : `bool` |
| `accept(Player $player)` : `bool` |
| `leave(Player $player)` : `void` |
| `getMembers(string $playerName)` : `array` |
| `getLeader(string $playerName)` : `?string` |
| `size(string $playerName)` : `int` |
| `isLeader(string $playerName)` : `bool` |

### `SeasonService`

`src/MiniGameBase/service/SeasonService.php`

| Метод |
|-------|
| `currentId()` : `string` |
| `statKey(string $stat)` : `string` |
| `info(CommandSender $sender)` : `void` |
| `startNew(CommandSender $sender, string $newId)` : `void` |
| `record(Player $player, string $stat, int $amount = 1)` : `void` |

### `SpectatorService`

`src/MiniGameBase/service/SpectatorService.php`

| Метод |
|-------|
| `enter(Player $player, Arena $arena)` : `bool` |
| `leave(Player $player)` : `bool` |
| `isSpectator(string $playerName)` : `bool` |
| `getArenaId(string $playerName)` : `?string` |
| `onQuit(Player $player)` : `void` |

### `StatsExporter`

`src/MiniGameBase/service/StatsExporter.php`

| Метод |
|-------|
| `tick()` : `void` |
| `export(string $event, array $payload)` : `void` |

### `ZoneService`

`src/MiniGameBase/service/ZoneService.php`

| Метод |
|-------|
| `startShrink(string $zoneId, Arena $arena, Vector3 $center, float $startRadius, float $endRadius, int $durationSec, float $damagePerSec = 1.0)` : `void` |
| `setFixed(string $zoneId, Arena $arena, Vector3 $center, float $radius, float $damagePerSec = 1.0)` : `void` |
| `stop(string $zoneId)` : `void` |
| `clearArena(string $arenaId)` : `void` |
| `getRadius(string $zoneId)` : `?float` |
| `isInside(string $zoneId, Vector3 $pos)` : `?bool` |
| `tickSecond(Arena $arena)` : `void` |

## Модули — публичные менеджеры / сервисы

### `Manager`

`src/MiniGameBase/modules/economy/manager/Manager.php`

| Метод |
|-------|
| `setMoney(string $nickname, float $amount)` : `bool` |
| `takeMoney(string $nickname, float $amount)` : `bool` |
| `getMoney(string $nickname)` : `float` |
| `transferMoney(string $fromPlayer, string $toPlayer, float $amount)` : `bool` |
| `hasMoney(string $nickname, float $amount)` : `bool` |
| `getTopPlayers(int $limit = 10)` : `array` |
| `getPlayerInfo(string $nickname)` : `array` |
| `claimDailyBonus(string $nickname)` : `array` |
| `formatMoney(float $amount)` : `string` |
| `parseMoney(string $amount)` : `?float` |
| `createShop(string $shopName, string $owner, string $itemId, string $itemName, ?float $buyPrice = NULL, ?float $sellPrice = NULL)` : `bool` |
| `canAfford(string $nickname, float $amount)` : `bool` |
| `paySalary(string $nickname, string $job, float $amount)` : `bool` |
| `addMoney(string $nickname, float $amount)` : `bool` |
| `getEconomyStats()` : `array` |
| `clearCache()` : `void` |
| `getDatabase()` : `DataBase` |

### `Manager`

`src/MiniGameBase/modules/auth/manager/Manager.php`

| Метод |
|-------|
| `needForAuth(int $playerId)` : `bool` |
| `getAuthStatus(int $playerId)` : `int` |
| `setAuthStatus(int $playerId, int $status)` : `void` |
| `unsetAuthStatus(int $playerId)` : `void` |
| `registerPlayer(Player $player, string $password)` : `void` |
| `setForAuth(Player $player)` : `void` |
| `validPassword(Player $player, string|null $password)` : `bool` |

### `AuthModule`

`src/MiniGameBase/modules/auth/AuthModule.php`

| Метод |
|-------|
| `getName()` : `string` |
| `getVersion()` : `string` |
| `getDescription()` : `string` |
| `getLoadPriority()` : `int` |
| `getDependencies()` : `array` |
| `getAuthManager()` : `Manager` |
| `getAuthDatabase()` : `DataBase` |
| `sendAuth(Player $player, string $nickname)` : `void` |
| `addPlayerTask(int $playerId, $task)` : `void` |
| `removePlayerTask(int $playerId)` : `void` |
| `getPlayerTasks()` : `array` |

### `Manager`

`src/MiniGameBase/modules/bossbar/manager/Manager.php`

| Метод |
|-------|
| `removeBossBar(Player $player)` : `bool` |
| `updateBossBar(Player $player, ?string $title = NULL, ?float $progress = NULL, ?int $color = NULL)` : `bool` |
| `showProgressBar(Player $player, string $title, int $duration, int $color = 3)` : `int` |
| `showBossBar(Player $player, string $title, float $progress = 1.0, int $color = 5)` : `int` |
| `showAnimatedBossBar(Player $player, array $frames, float $frameDelay = 0.5, bool $loop = true)` : `int` |
| `hasBossBar(Player $player)` : `bool` |
| `getBossBarId(Player $player)` : `?int` |
| `removeAllBossBars()` : `void` |
| `getStats()` : `array` |
| `showGameBossBar(Player $player, string $gameType, array $data = [])` : `int` |
| `showTimer(Player $player, string $titleTemplate, int $seconds, int $color = 2)` : `int` |

### `Manager`

`src/MiniGameBase/modules/scoreboard/manager/Manager.php`

| Метод |
|-------|
| `updateScoreboardContent(Player $player)` : `void` |
| `getPlayerData(string $playerName)` : `array` |
| `setPlayerData(string $playerName, string $key, mixed $value)` : `void` |
| `showScoreboard(Player $player, string $template)` : `bool` |
| `removeScoreboard(Player $player)` : `void` |
| `registerTemplate(string $name, array $template)` : `void` |
| `getTemplates()` : `array` |
| `hasScoreboard(Player $player)` : `bool` |
| `getPlayerScoreboardType(Player $player)` : `?string` |
| `removeAllScoreboards()` : `void` |
| `getStats()` : `array` |

### `Manager`

`src/MiniGameBase/modules/punish/manager/Manager.php`

| Метод |
|-------|
| `unbanPlayer(string $nickname, string $admin, ?string $reason = NULL)` : `bool` |
| `unmutePlayer(string $nickname, string $admin, ?string $reason = NULL)` : `bool` |
| `getPlayerInfo(string $nickname)` : `array` |
| `removeWarning(int $warningId, string $admin)` : `bool` |
| `getAdminLogs(string $admin, int $limit = 50)` : `array` |
| `getStats()` : `array` |
| `checkForSpam(Player $player, string $message)` : `bool` |
| `warnPlayer(string $nickname, string $reason, string $admin, string $violationType = 'other')` : `bool` |
| `banPlayer(string $nickname, string $reason, string $admin, ?int $duration = NULL)` : `bool` |
| `mutePlayer(string $nickname, string $reason, string $admin, int $duration)` : `bool` |
| `kickPlayer(string $nickname, string $reason, string $admin)` : `bool` |
| `getViolationTypes()` : `array` |
| `notifyAdmins(string $message)` : `void` |
| `hasPermission(Player $player, string $permission)` : `bool` |

### `Manager`

`src/MiniGameBase/modules/timeouts/manager/Manager.php`

| Метод |
|-------|
| `isBanned(string $nickname)` : `bool` |
| `getPlayerTimeouts(string $nickname)` : `array` |
| `getPlayerHistory(string $nickname, int $limit = 10)` : `array` |
| `checkPlayerOnJoin(Player $player)` : `bool` |
| `checkPlayerChat(Player $player)` : `bool` |
| `isMuted(string $nickname)` : `bool` |
| `parseDuration(string $duration)` : `?int` |
| `getStats()` : `array` |
| `getPresetDurations()` : `array` |
| `addBan(string $nickname, string $admin, string $reason, ?int $duration = NULL)` : `bool` |
| `banPlayer(string $nickname, string $reason, string $admin, ?int $duration = NULL, ?string $ip = NULL, ?string $cid = NULL)` : `bool` |
| `addMute(string $nickname, string $admin, string $reason, ?int $duration = NULL)` : `bool` |
| `mutePlayer(string $nickname, string $reason, string $admin, int $duration)` : `bool` |
| `addKick(string $nickname, string $admin, string $reason)` : `bool` |
| `kickPlayer(string $nickname, string $reason, string $admin)` : `bool` |
| `removeBan(string $nickname, string $admin, ?string $reason = NULL)` : `bool` |
| `unbanPlayer(string $nickname, string $admin, ?string $reason = NULL)` : `bool` |
| `removeMute(string $nickname, string $admin, ?string $reason = NULL)` : `bool` |
| `unmutePlayer(string $nickname, string $admin, ?string $reason = NULL)` : `bool` |

### `Manager`

`src/MiniGameBase/modules/npc/manager/Manager.php`

| Метод |
|-------|
| `loadAllNPCs()` : `void` |
| `spawnNPC(array $npcData)` : `?Entity` |
| `unloadAllNPCs()` : `void` |
| `createNPC(string $name, string $displayName, Vector3 $position, World $world, string $npcType = 'default', array $options = [])` : `?int` |
| `deleteNPC(int $npcId)` : `bool` |
| `getNPCByEntityId(int $entityId)` : `?array` |
| `handleNPCInteraction(Player $player, Entity $entity)` : `void` |
| `getNPCTypes()` : `array` |
| `registerNPCType(string $type, array $config)` : `void` |
| `getLoadedNPCs()` : `array` |

### `NpcAiService`

`src/MiniGameBase/modules/npcai/NpcAiService.php`

| Метод |
|-------|
| `start()` : `void` |
| `shutdown()` : `void` |
| `catalog()` : `NpcCatalog` |
| `brain()` : `NpcAiBrain` |
| `spawner()` : `NpcSpawner` |
| `spawnAt(World $world, Vector3 $pos, NpcSpawnSettings $settings)` : `?Entity` |
| `spawnCatalog(World $world, Vector3 $pos, string $definitionId, array $overrides = [])` : `?Entity` |
| `despawn(Entity $entity)` : `void` |
| `despawnNear(World $world, Vector3 $pos, float $radius = 12.0)` : `int` |

### `TeamGameService`

`src/MiniGameBase/modules/teamgame/TeamGameService.php`

| Метод |
|-------|
| `cfg(?Arena $arena = null)` : `array` |
| `shutdown()` : `void` |
| `shop()` : `ShopService` |
| `sessionOf(Arena $arena)` : `?ArenaSession` |
| `giveWaitingLobbyItems(Arena $arena, Player $player)` : `void` |
| `selectLobbyTeam(Arena $arena, Player $player, int $teamId)` : `bool` |
| `clearLobbyTeamPick(Arena $arena, Player $player)` : `void` |
| `lobbyTeamIds(Arena $arena)` : `array` |
| `isLobbyTeamWool(Item $item)` : `?int` |
| `sessions()` : `array` |
| `ensureListener()` : `void` |
| `startMatch(Arena $arena)` : `void` |
| `startClocks(Arena $arena)` : `void` |
| `cancelClock(Arena $arena)` : `void` |
| `tickSecond(Arena $arena)` : `void` |
| `onPlayerDeath(Arena $arena, Player $player, ?Player $killer = null)` : `void` |
| `scheduleGameEnd(Arena $arena, ?Player $winnerPlayer = null, ?Team $winnerTeam = null)` : `void` |
| `onGameEnd(Arena $arena, ?Player $winnerPlayer = null, ?Team $winnerTeam = null)` : `void` |
| `openItemShop(Arena $arena, Player $player)` : `void` |
| `openUpgradeShop(Arena $arena, Player $player)` : `void` |
| `broadcast(Arena $arena, string $message)` : `void` |
| `lang(Arena $arena, string $key, array $params = [], ?string $fallback = null)` : `string` |

### `ShopService`

`src/MiniGameBase/modules/teamgame/shop/ShopService.php`

| Метод |
|-------|
| `openItemShop(Player $player, ArenaSession $session, array $cfg)` : `void` |
| `openCategory(Player $player, ArenaSession $session, Team $team, string $cat, array $items, array $cfg)` : `void` |
| `buyItem(Player $player, ArenaSession $session, Team $team, array $row, array $cfg)` : `void` |
| `openUpgradeShop(Player $player, ArenaSession $session, array $cfg)` : `void` |
| `buyUpgrade(Player $player, ArenaSession $session, Team $team, array $row, array $cfg)` : `void` |
| `buyTrap(Player $player, ArenaSession $session, Team $team, array $row, array $cfg)` : `void` |
| `takeCurrency(Player $player, string $currency, int $amount, array $cfg = [])` : `bool` |
| `refund(Player $player, string $currency, int $amount, array $cfg = [])` : `void` |

### `Manager`

`src/MiniGameBase/modules/floatingtext/manager/Manager.php`

| Метод |
|-------|
| `removeAllFloatingTexts()` : `void` |

### `Manager`

`src/MiniGameBase/modules/groups/manager/Manager.php`

| Метод |
|-------|
| `isAccess($player)` : `bool` |
| `getDefaultGroup()` : `string` |
| `getPrioritize(Player $player, string $groupName, bool $message = true)` : `bool` |
| `assignGroupFromCommand(CommandSender $sender, string $recipient, string $group_name, $time_stamp, string $time_string, bool $ignore)` : `void` |
| `getRconByGroupPrioritize(string $group_first, string $groupName)` : `bool` |
| `getRconPrioritize(string $nickname, string $groupName)` : `bool` |

### `Manager`

`src/MiniGameBase/modules/chat/manager/Manager.php`

| Метод |
|-------|
| `setChat(Player $player, string $message)` : `string` |
| `getTitle(string $nickname)` : `string` |
| `getGroupColor(string $nickname)` : `string` |
| `setDisplay(Player $player)` : `void` |
| `getTag(Player $player, string $nickname)` : `string` |
| `getPlayerPlatform(Player $player)` : `string` |
| `setAuthDisplay(Player $player, string $status)` : `void` |
| `setNameTagMessage(Player $player, string $message, int $type)` : `void` |
| `getLoader()` : `ChatModule` |

### `Manager`

`src/MiniGameBase/modules/cosmetics/manager/Manager.php`

| Метод |
|-------|
| `loadCosmetics()` : `void` |
| `saveAllPlayerData()` : `void` |

### `Manager`

`src/MiniGameBase/modules/vkinteract/manager/Manager.php`

| Метод |
|-------|
| `sendAdminMessage(string $message, ?string $sender = NULL, ?array $additionalData = NULL)` : `bool` |
| `sendBanNotification(string $bannedPlayer, string $admin, string $reason, int $duration = 0)` : `bool` |
| `sendUnbanNotification(string $unbannedPlayer, string $admin)` : `bool` |
| `sendMuteNotification(string $mutedPlayer, string $admin, string $reason, int $duration)` : `bool` |
| `sendGroupNotification(string $player, string $group, string $admin, int $duration = 0)` : `bool` |
| `sendCustomMessage(string $message, string $type = 'custom', ?string $sender = NULL)` : `bool` |
| `updateSettings(array $settings)` : `bool` |
| `getSettings()` : `array` |
| `testConnection()` : `bool` |
| `getStats()` : `array` |
| `isEnabled()` : `bool` |
| `setEnabled(bool $enabled)` : `void` |
| `sendAdmins(string $message)` : `bool` |

### `Manager`

`src/MiniGameBase/modules/individualids/manager/Manager.php`

| Метод |
|-------|
| `getNicknameFromIdentifier(int $identifier)` : `?string` |
| `hasID(int $id)` : `bool` |
| `getNicknameFromID(int $id)` : `?string` |
| `isValidID(string $identifier)` : `bool` |
| `getOrCreateID(string $nickname)` : `int` |
| `getIDFromNickname(string $nickname)` : `?int` |
| `hasAccount(string $nickname)` : `bool` |
| `registerAccount(string $nickname)` : `bool` |
| `searchPlayers(string $query, int $limit = 10)` : `array` |
| `exportData()` : `array` |
| `getAllAccounts(int $offset = 0, int $limit = 100)` : `array` |
| `getSystemStats()` : `array` |
| `clearCache()` : `void` |
| `maintainCache()` : `void` |
| `getDataBase()` : `DataBase` |

## События

### `ArenaEndEvent`

`src/MiniGameBase/event/ArenaEndEvent.php` · —

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getWinner()` : `?Player` |

### `ArenaGameStartingEvent`

`src/MiniGameBase/event/ArenaGameStartingEvent.php` · Cancellable

| Метод |
|-------|
| `getArena()` : `Arena` |
| `isCancelled()` : `bool` |
| `setCancelled(bool $value = true)` : `void` |
| `cancel()` : `void` |

### `ArenaPhaseChangeEvent`

`src/MiniGameBase/event/ArenaPhaseChangeEvent.php` · —

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getFrom()` : `string` |
| `getTo()` : `string` |

### `ArenaStartEvent`

`src/MiniGameBase/event/ArenaStartEvent.php` · —

| Метод |
|-------|
| `getArena()` : `Arena` |

### `MatchSecondEvent`

`src/MiniGameBase/event/MatchSecondEvent.php` · —

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getElapsedSeconds()` : `int` |

### `MiniGameBaseReadyEvent`

`src/MiniGameBase/event/MiniGameBaseReadyEvent.php` · —

| Метод |
|-------|
| `getMain()` : `Main` |

### `PlayerDeathInArenaEvent`

`src/MiniGameBase/event/PlayerDeathInArenaEvent.php` · Cancellable

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getVictim()` : `Player` |
| `getPlayer()` : `Player` |
| `getKiller()` : `?Player` |
| `getDeathCause()` : `string` |
| `isCancelled()` : `bool` |
| `setCancelled(bool $value = true)` : `void` |
| `cancel()` : `void` |

### `PlayerDequeuedEvent`

`src/MiniGameBase/event/PlayerDequeuedEvent.php` · —

| Метод |
|-------|
| `getPlayer()` : `Player` |
| `getGameType()` : `string` |
| `getReason()` : `string` |

### `PlayerEnterMatchSpectatorEvent`

`src/MiniGameBase/event/PlayerEnterMatchSpectatorEvent.php` · Cancellable

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getPlayer()` : `Player` |
| `getReason()` : `string` |
| `isCancelled()` : `bool` |
| `setCancelled(bool $value = true)` : `void` |
| `cancel()` : `void` |

### `PlayerJoinArenaEvent`

`src/MiniGameBase/event/PlayerJoinArenaEvent.php` · Cancellable

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getPlayer()` : `Player` |
| `isCancelled()` : `bool` |
| `setCancelled(bool $value = true)` : `void` |
| `cancel()` : `void` |

### `PlayerLeaveArenaEvent`

`src/MiniGameBase/event/PlayerLeaveArenaEvent.php` · —

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getPlayer()` : `Player` |
| `isDisconnect()` : `bool` |

### `PlayerLeaveMatchSpectatorEvent`

`src/MiniGameBase/event/PlayerLeaveMatchSpectatorEvent.php` · —

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getPlayer()` : `Player` |
| `getReason()` : `string` |

### `PlayerQueuedEvent`

`src/MiniGameBase/event/PlayerQueuedEvent.php` · —

| Метод |
|-------|
| `getPlayer()` : `Player` |
| `getGameType()` : `string` |

### `VoteStartEvent`

`src/MiniGameBase/event/VoteStartEvent.php` · —

| Метод |
|-------|
| `getArena()` : `Arena` |

### `TeamObjectiveBreakEvent`

`src/MiniGameBase/event/teamgame/TeamObjectiveBreakEvent.php` · —

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getTeam()` : `Team` |
| `getPlayer()` : `Player` |
| `getX()` : `int` |
| `getY()` : `int` |
| `getZ()` : `int` |
| `isOwnTeam()` : `bool` |

### `TeamObjectiveDestroyEvent`

`src/MiniGameBase/event/teamgame/TeamObjectiveDestroyEvent.php` · —

| Метод |
|-------|
| `getArena()` : `Arena` |
| `getTeam()` : `Team` |
| `getDestroyer()` : `?Player` |

