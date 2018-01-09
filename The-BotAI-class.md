The class `sc2.BotAI` provides a base class for all bots. It has methods that can be used to access the game state and to create actions.

# Properties

Name                    | Description
------------------------|-------------
`player_id`             | Unique identifier
`race`                  | One of Zerg, Terran, Protoss
`game_info`             | A [GameInfo](https://github.com/Dentosal/python-sc2/blob/master/sc2/game_info.py) object, contains the initial/static game state
`enemy_start_locations` | A list of possible enemy start locations
`known_enemy_structures`| A list of known (and seen) enemy structures


# Methods

Name                                     | Description
-----------------------------------------|-------------
`can_afford(item_id)`                    | Tests if the player has enough resources to build a unit, research an upgrade or use an ability. It takes previous actions (ran through `self.do`) into account
`select_build_worker(pos, force=False)`  | Selects a free worker near to `pos` to build a building. If `force=True` is given, selects a random worker if no workers were available or near
`async can_place(building, position)`    | `async` Tests if the building can be placed in the given position
`async find_placement(building, near, max_distance=20)` | `async` Finds a free position for the building near the given position
`already_pending(unit_type)`             | Tests if the given unit is already being built
`async build(building, near, max_distance=20, unit=None)` | `async` Builds the given building, optionally with the specific unit given
`async do(action)`                       | `async` Executes the given action
`async def chat_send(self, message)`     | `async` Sends a chat message. Emojis are supported, so e.g. writing `(glhf)` is replaced with the glhf-icon

# Inheriting

The `async on_step` methods, called on every game step, must be overridden. Optionally you can also override the `on_start` method to set up your class after the game data has been given.
