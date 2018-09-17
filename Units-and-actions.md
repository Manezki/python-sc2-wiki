Everything dynamic in StarCraft 2 is a unit. Walking creatures, buildings, and even mineral fields are all units. Units are passed you your `async def on_step(self, iteration)` method inside the `self.state` property, as `self.state.units`, which is a group of all known units in the game. Your own units are also available as `self.units`.

# Unit groups / Units

Unit group ([sc2.units.Units](https://github.com/Dentosal/python-sc2/blob/master/sc2/units.py)) is an ordered set-like structure containing all units. Normal set operations (`|`, `&`, `-`) can be used with unit groups.

## Properties

Name             | Description
-----------------|-------------
`amount`         | `int` Number of units in the group
`empty`          | `bool` Is the group empty
`exists`         | `bool` Is the group not empty
`center`         | `Point2` Returns the central point of a unit group, assumes `self.amount` > 0
`tags(pos)`      | `Set[int]` Returns a set of all unit tags in this unit group
`closest_distance_to(pos)` | `float` distance to the closest in the group to `position`
`furthest_distance_to(pos)` | `float` distance to the furthest in the group to `position`

## Unit selectors

Return a single unit

Name                  | Description
----------------------|-------------
`first`               | The first unit in the group
`random`              | A random unit in the group
`random_of(other)`    | A random unit in the group, or `other` in case that the group is empty
`closest_to(position)`| The unit closest to `position`
`furthest_to(position)`| The unit furthest to `position`
`find_by_tag`         | Find unit by tag, returns None if not found
`by_tag`              | Find unit by tag, throws KeyError if not found

## Unit group selectors

Return a unit group of type `Units`

### Misc

Name                             | Description
---------------------------------|-------------
`take(n)`                        | Takes first `n` units in the group, or all if the group is smaller
`take(n, True)`                  | Takes first `n` units in the group (requires the group to contain at least that many units)
`random_group_of(n)`             | A random subgroup of n units

### Filters

Name                              | Description
----------------------------------|-------------
`filter(predicate)`               | Selects all units for which predicate returns True
`closer_than(distance, position)` | All units closer than `distance` to the `position`
`further_than(distance, position)`| All units further than `distance` to the `position`
`tags_in(set)`                    | Returns all units whose tags are in `set`. Can be used as the reverse of `Units.tags`
`tags_not_in(set)`                | Returns all units whose tags are not in `set`
`of_type(set)`                    | Returns all units that are of type `UnitTypeId` in the `set`. E.g. set = {UnitTypeId.MARINE, UnitTypeId.MARAUDER}
`exclude_type(set)`               | Returns all units that are not of type `UnitTypeId` in the `set`. E.g. set = {UnitTypeId.OVERLORD, UnitTypeId.LARVA, UnitTypeId.EGG}



### Property filters

Name                       | Description
---------------------------|-------------
`ready`                    | Is ready (building done, unit fully spawned)
`not_ready`                |
`noqueue`                  | Command queue is empty 
`idle`                     | No assigned action
`owned`                    | Owned by this player
`enemy`                    | Owned by opponent
`structure`                | Is structure
`not_structure`            |
`flying`                   | Is flying
`not_flying`               | Is not flying
`gathering`                | Is mining from minerals or gas
`returning`                | Is returning from minerals or gas
`collecting`               | Is mining or returning from minerals or gas
`mineral_field`            | Is mineral field
`vespene_geyser`           | Is vespene geyser
`in_attack_range_of(unit)` | Returns all units that the unit in the argument can shoot at

### Sorting

Name                             | Description
---------------------------------|-------------
`prefer_idle`                    | `property` Prefer idle units
`prefer_close_to(position)`      | Prefer units closer to the `position`
`sorted(keyfn, reverse=False)`   | Sorts units by keyfn

# Unit

A single unit ([sc2.unit.Unit](https://github.com/Dentosal/python-sc2/blob/master/sc2/unit.py)) has properties and actions (a.k.a. commands)

## Properties

Name               | Description
-------------------|-------------
`_proto `          | Raw unit info
`type_id`          | `UnitTypeId` of the unit (the enum value)
`_type_data`       | `UnitTypeData` of the unit (more info about the unit type, see `self._game_info`)
`is_snapshot`      | `bool` Snapshot, a previously seen building
`is_visible`       | `bool` Currently visible
`alliance`         | Alliance status
`is_mine`          | `bool` Owned by current player
`is_enemy`         | `bool` Enemy-owned unit
`tag`              | `int` Unique integer identifier
`owner_id`         | `player_id` of the owner
`position`         | `Point2` Current position
`position3d`       | `Point3` Current position
`facing`           | `float` the direction the unit is facing. east is 0, north is 0.5rad
`distance_to(p)`   | `float` distance to unit/position
`radius`           | `float` the unit size
`detect_range`     | `float` How far this unit detects clocked or burrowed units
`radar_range`      | `float` How far this unit sees
`build_progress`   | `float` Build progress, range [0.0, 1.0]
`is_ready`         | `bool` Build progress == 1
`cloak`            | `Cloak` Cloak status
`is_blip`          | `bool` Detected by watch tower
`is_powered`       | `bool` Some Protoss buildings must be in the power field to work
`is_burrowed`      | `bool`
`is_flying`        | `bool`
`is_structure`     | `bool` Note: some structures can walk or fly
`is_light`         | `bool`
`is_armored`       | `bool`
`is_biological`    | `bool`
`is_mechanical`    | `bool`
`is_robotic`       | `bool`
`is_massive`       | `bool`
`is_psionic`       | `bool`
`is_mineral_field` | `bool`
`is_vespene_geyser`| `bool`
`tech_alias`       | `None` or `List[UnitTypeId` E.g. for HIVE this returns [UnitTypeId.HATCHERY, UnitTypeId.LAIR], for SCV it returns None
`unit_alias`       | `None` or `List[UnitTypeId` E.g. for FLYINGORBITALCOMMAND this returns UnitTypeId.COMMANDCENTER, for SCV it returns None
`race`             | `Race` e.g. `Race.Terran`, `Race.Random`
`health`           | Current health
`health_max`       | Max health
`health_percentage`| Current health / max health
`shield`           | Current shield
`shield_max`       | Max shield
`shield_percentage`| Current shield / max shield
`energy`           | Current energy
`energy_max`       | Max energy
`energy_percentage`| Current energy / max energy
`mineral_contents` | `int` Amount of minerals
`vespene_contents` | `int` Amount of vespene
`weapon_cooldown`  | `float` how many frames remaining until the unit can attack again
`cargo_size`       | `int` How much cargo this unit uses up in cargo_space
`has_cargo`        | `bool` If this unit has units loaded
`cargo_used`       | `int` How much cargo space is used (some units take up more than 1 space)
`cargo_max`        | `int` How much cargo space is totally available - CC: 5, Bunker: 4, Medivac: 8 and Bunker can only load infantry, CC only SCVs
`passengers`       | `Set[PassengerUnit]` Units inside a Bunker, CommandCenter, Nydus, Medivac, WarpPrism, Overlord
`passengers_tags`  | `Set[int]` Tags of the passengers
`can_attack_ground`| `bool`
`ground_dps`       | `float` Does not include upgrades
`ground_range`     | `range` Does not include upgrades
`can_attack_air`   | `bool`
`air_dps`          | `float` Does not include upgrades
`air_range`        | `range` Does not include upgrades
`target_in_range(unit)`| `bool` Includes the target's radius when calculating distance to target
`sight_range`      | `float`
`movement_speed`   | `float` Does not include effects
`is_carrying_minerals`| `bool`
`is_carrying_vespene`| `float`
`is_selected`      |
`orders`           | `List[UnitOrder]` Order queue
`noqueue`          | `bool` Is the order queue empty
`is_moving`        | `bool` If the unit is moving
`is_attacking`     | `bool` If the unit is attacking
`is_gathering`     | `bool` If the unit is mining minerals or gas
`is_returning`     | `bool` If the unit is returning from minerals or gas
`is_collecting`    | `bool` If the unit is mining or returning minerals or gas
`order_target`     | `None` or `int` or `Point2` If the unit is not idle, returns the first order target (if targeting a unit, returns its unit tag, else returns target point)
`is_idle`          | No current action, same as noqueue
`add_on_tag`       | `int` Returns 0 if this unit has no addon, else returns the addon's tag
`add_on_land_position`| `Point2` Assuming this unit is an addon, this returns the exact position a terran structure has to land to attach to the addon
`has_add_on`       | `bool` if `add_on_tag` == 0
`assigned_harvesters`| `int` how many workers are mining minerals (if unit is a townhalls) or mining gas (if unit is refinery/extractor/assimilator)
`ideal_harvesters`| `int` how many workers ideally should be mining minerals (if unit is a townhalls) or mining gas (if unit is refinery/extractor/assimilator)
`surplus_harvesters`| `int` assigned_harvesters - ideal_harvesters returns negative number if too few are mining
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
`research(upgrade)`| Research an upgrade
`has_buff(buff)`   | `bool` If the unit has a buff
`warp_in(unit, pos)`| Assumes the unit is a warp gate, tries to warp in a protoss unit
`attack()`         | Attack
`gather()`         | Gather a resource
`return_resource()`| Return a resource
`move()`           | Move to
`hold_position()`  | Hold position
`stop()`           | Stop current action
`(action)`         | `__call__` Use the given ability