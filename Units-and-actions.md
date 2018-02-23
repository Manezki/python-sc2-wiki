Everything dynamic in StarCraft 2 is a unit. Walking creatures, buildings, and even mineral fields are all units. Units are passed you your `async def on_step(self, state, iteration)` method inside the `state` parameter, as `state.units`, which is a group of all known units in the game.

# Unit groups

Unit group ([sc2.units.Units](https://github.com/Dentosal/python-sc2/blob/master/sc2/units.py)) is an ordered set-like structure containing all units. Normal set operations (`|`, `&`, `-`) can be used with unit groups.

## Properties

Name             | Description
-----------------|-------------
`amount`         | Number of units in the group
`empty`          | Is the group empty
`exists`         | Is the group not empty

## Unit selectors

Return a sinle unit

Name                  | Description
----------------------|-------------
`first`               | `property` The first unit in the group
`random`              | `property` A random unit in the group
`take(n)`             | Takes first `n` units in the group, or all if the group is smaller
`random_of(other)`    | A random unit in the group, or `other` in case that the group is empty
`closest_to(position)`| The unit closest to `position`
`find_by_tag`         | Find unit by tag, returns None if not found
`by_tag`              | Find unit by tag, throws KeyError if not found

## Unit group selectors

Return a `UnitGroup`

### Misc

Name                             | Description
---------------------------------|-------------
`take(n, True)`                  | Takes first `n` units in the group (requires the group to contain at least that many units)
`random_group_of(n)`             | A random subgroup of n units

### Filters

Name                             | Description
---------------------------------|-------------
`filter(predicate)`              | Selects all units for which predicate returns True
`closer_than(distance, position)`| All units closer than `distance` to the `position`

### Property filters

Name            | Description
----------------|-------------
`ready`         | Is ready (building done, unit fully spawned)
`not_ready`     |
`noqueue`       | Command queue is empty 
`idle`          | No assigned action
`owned`         | Owned by this player
`enemy`         | Owned by opponent
`structure`     | Is structure
`not_structure` |
`mineral_field` | Is mineral field
`vespene_geyser`| Is vespene geyser

### Sorting

Name                             | Description
---------------------------------|-------------
`prefer_idle`                    | `property` Prefer idle units
`prefer_close_to(position)`      | Prefer units closer to the `position`
`sorted(keyfn, reverse=False)`   | Sorts units by keyfn

# Units

A single unit ([sc2.unit.Unit](https://github.com/Dentosal/python-sc2/blob/master/sc2/unit.py)) has properties and actions (a.k.a. commands)

## Properties

Name               | Description
-------------------|-------------
`is_snapshot`      | Snapshot, a previously seen building
`is_visible`       | Currently visible
`alliance`         | Alliance status
`is_mine`          | Owned by current player
`is_enemy`         | Enemy-owned unit
`tag`              | Unique integer identifier
`owner_id`         | `player_id` of the owner
`position`         | Current position
`facing`           | 
`radius`           |
`detect_range`     | How far this unit detects clocked or burrowed units
`radar_range`      | How far this unit sees
`build_progress`   | Build progress, range [0.0, 1.0]
`is_ready`         | Build progress ready
`cloak`            | Cloak status
`is_blip`          | Detected by watch tower
`is_powered`       | Some Protoss buildings must be in the power field to work
`is_burrowed`      |
`is_flying`        |
`is_structure`     | Note: some structures can walk or fly
`is_mineral_field` |
`is_vespene_geyser`|
`health`           | Current health
`health_max`       | Max health
`shield`           | Current shield
`shield_max`       | Max shield
`energy`
`mineral_contents` | Amount of minerals
`vespene_contents` | Amount of vespene
`is_selected`      |
`orders`           | Order queue
`noqueue`          | Is the order queue empty
`is_idle`          | No current action
`name`             | Unit type name
`assigned_harvesters` | Harvester units assigned to this unit
`ideal_harvesters` | Ideal number of harvester units for this unit

## Actions

Action returns a [`sc2.unit_command.UnitCommand`](https://github.com/Dentosal/python-sc2/blob/master/sc2/unit_command.py). Note that actions are not executed by calling these methods, they just create the action that can be executed by `await self.do(action)` in your bot class.

All these methods take additional parameters: a (sometimes optional) `target` and a (fully optional) `queue` (defaulting to False). Target is required by some commands, and disallowed in others. It may be either point or unit when defined. If `queue` is `True` then the command is queued after the previous actions, otherwise it overrides the current action.

Name               | Description
-------------------|-------------
`train(unit)`      | Train a new unit
`build(unit)`      | Build a building
`attack()`         | Attack
`gather()`         | Gather a resource
`move()`           | Move to
`hold_position()`  | Hold position
`stop()`           | Stop current action
`(action)`         | `__call__` Use the given ability