The class `sc2.BotAI` provides a base class for all bots. It has methods that can be used to access the game state and to create actions.

# Properties

Name                    | Description
------------------------|-------------
`player_id`             | Unique identifier (int: 1 or 2)
`race`                  | Your bot's race. One of `Race.Zerg`, `Race.Terran`, `Race.Protoss`, `Race.Random`
`enemy_race`            | The opponent race
`time`                  | The game time in seconds (float)
`start_location`        | A [Point2](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/position.py#L71) of your spawn location
`enemy_start_locations` | A `list` of possible enemy start locations as `Point2`
`known_enemy_units`     | A `list` of known (and seen) enemy units, including structures
`known_enemy_structures`| A `list` of known (and seen) enemy structures
`main_base_ramp`        | Your main base ramp as [Ramp](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/game_info.py#L12) object. Right now there are terran building positions for the ramp available for supply depots and barracks placement.
`expansion_locations`   | A `dict` with key: `Point2` and value: `Units` of each bases' resources (including vespene gas) with the `key` being the center of these
`owned_expansions`      | A `list` of expansions owned by the player
&nbsp;|
`workers`               | Own workers as [Units](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/units.py#L8) object
`units`                 | Own units as [Units](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/units.py#L8) object
`townhalls`             | Own townhalls as [Units](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/units.py#L8) object
`geysers`               | Own geysers as [Units](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/units.py#L8) object
`minerals`              | Amount of minerals (int)
`vespene`               | Amount of vespene gas (int)
`supply_used`           | Current supply used by units
`supply_cap`            | Current supply cap limited by bases and supply depots, pylons, overlords
`supply_left`           | `supply_cap` - `supply_used` 
&nbsp;|
`_game_info`            | A [GameInfo](https://github.com/Dentosal/python-sc2/blob/master/sc2/game_info.py) object, contains the initial/static game state (for more info see below)
`_game_data`            | A [GameData](https://github.com/Dentosal/python-sc2/blob/master/sc2/game_data.py) object, contains data about units, abilities, upgrades, buffs and effects
`state`                 | A [GameState](https://github.com/Dentosal/python-sc2/blob/master/sc2/game_state.py) object, contains data about the current state of the game (fore more info see below)

# Methods

Name                                     | Description
-----------------------------------------|-------------
`can_afford(item_id)`                    | Tests if the player has enough resources to build a unit, research an upgrade or use an ability. It takes previous actions (ran through `self.do`) into account
`select_build_worker(pos, force=False)`  | Selects a free worker near to `pos` to build a building. If `force=True` is given, selects a random worker if no workers were available or near
`async can_place(building, position)`    | `async` Tests if the building can be placed in the given position
`async find_placement(building, near, max_distance=20)` | `async` Finds a free position for the building near the given position
`already_pending(unit_type)`             | Tests if the given unit is already being built
`async build(building, near, max_distance=20, unit=None)` | `async` Builds the given building, optionally with the specific unit given. Use the `near` param to specify a structure or a position near to which this building is to be built.
`async do(action)`                       | `async` Executes the given action. Returns error object if fails.
`async do_actions(list_of_actions)`      | `async` Executes the given actions. Returns no errors. It is recommended to use this instead of `await self.do(action)` when your bot starts to run very slowly
`async def chat_send(self, message)`     | `async` Sends a chat message. Emojis are supported, so e.g. writing `(glhf)` is replaced with the glhf-icon

# Inheriting

The `async on_step` methods, called on every game step, must be overridden. Optionally you can also override the `on_start` method to set up your class after the game data has been given. The `on_end` method will be called after a game has successfully (without crashing) ended.

# Race-specific methods and properties

## Protoss

Name                                     | Description
-----------------------------------------|-------------
`state.psionic_matrix.covers(pos)`       | Check psionic matrix coverage


# The GameInfo Class
## Properties
Name                    | Description
------------------------|-------------
`_proto`                | The raw data from the protocol
`players`               | A `list` of players as [Player](https://github.com/Dentosal/python-sc2/blob/master/sc2/player.py) object
`pathing_grid`          | A [PixelMap](https://github.com/Dentosal/python-sc2/blob/master/sc2/pixel_map.py) object containing data of possible unit movement locations
`terrain_height`        | A [PixelMap](https://github.com/Dentosal/python-sc2/blob/master/sc2/pixel_map.py) object containing data of terrain height. This is not exactly the same as the Z-location of a `unit.position3d`
`placement_grid`        | A [PixelMap](https://github.com/Dentosal/python-sc2/blob/master/sc2/pixel_map.py) object containing data of possible building placement locations (1x1 grid positions)
`playable_area`         | A [Rect](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/position.py#L173) object
`map_ramps`             | A `list` of ramps as [Ramp](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/game_info.py#L12) object. See `main_base_ramp`
`player_races`          | A `dict` of player id mapped to their `Race`
`start_locations`       | A `list` of start locations as `Point2`
`player_start_location` | Same as from the bot class `self.start_location`
`map_center`            | The center of the map as `Point2`


# The GameInfo Class
## Properties
Name                    | Description
------------------------|-------------
`actions`               | A `list` of sucessful unit actions since last game loop
`action_errors`         | A `list` of unsucessful unit actions since last game loop
`observation`           | The raw observation from the API
`player_result`         | The player result at the end of the game. Recommendation: use the `self.on_end()` function instead.
`chat`                  | A list of chat messages sent from the opponent
`common`                | A [Common](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/game_state.py#L51) object that will be extracted each game loop to `self.minerals`, `self.vespene` and the supply values
`psionic_matrix`        | A `list` of [PsionicMatrix](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/power_source.py#L25) objects for checking if a protoss building is powered
`game_loop`             | An integer of the current game loop. By default, this increments by 8 every `iteration` which means every game second around 3 iterations will be executed
`score`                 | A [ScoreDetails](https://github.com/Dentosal/python-sc2/blob/master/sc2/score.py) object containing up to date stats of income, spending and destruction (resources related)
`abilities`             | A `list` of available abilities this game loop from the selected units (not useful for this API)
`destructables`         | A `Units` object of all the destructable rocks (excluding the one next to the main ramp)
`units`                 | A `Units` object of all units (own, enemy and neutral)
`blips`                 | A `set` of [Blip](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/game_state.py#L11) objects (units detected by own terran sensor towers), more limited information than normal enemy units
`visibility`            | A [PixelMap](https://github.com/Dentosal/python-sc2/blob/master/sc2/pixel_map.py) object containing data about if you have (or had) vision to a position
`creep`                 | A [PixelMap](https://github.com/Dentosal/python-sc2/blob/master/sc2/pixel_map.py) object containing data about if there is creep on a position
`dead_units`            | A `set` of unit tags (`int`) that died last game loop
`effects`               | A `set` of [EffectData](https://github.com/Dentosal/python-sc2/blob/f045e9883f9083ee94a2da4631a4a637c568a734/sc2/game_state.py#L69) objects (e.g. high templar storms, ravager biles)
`upgrades`              | A `set` of `UpgradeId` enum entries that have been researched
`mineral_field`         | Equivalent to using `self.state.units.mineral_field`: A `Units` object containing all mineral fields
`vespene_geyser`        | Equivalent to using `self.state.units.vespene_geyser`: A `Units` object containing all vespene geysers fields
