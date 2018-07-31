# Should I use the [pysc2](https://github.com/deepmind/pysc2) or [python-sc2](https://github.com/Dentosal/python-sc2) library?
If you plan on making scripted hardcoded bots, python-sc2 is simpler in its syntax.

If you want to make a machine learning bot, pysc2 is probably the better choice since it uses the UI and feature layers (spatial interfaces).

# How do I do \<something\>?
Feel free to take a look at [the protoss/terran/zerg examples](https://github.com/Dentosal/python-sc2/tree/master/examples).

# What do I have access to in the step() function?
You have access to:

- all the basic BotAI class properties and functions listed [here](https://github.com/Dentosal/python-sc2/blob/master/sc2/bot_ai.py) and [here](https://github.com/Dentosal/python-sc2/blob/master/sc2/bot_ai.py#L386) through `self.<property_name>`, 

- all [the game state variables](https://github.com/Dentosal/python-sc2/blob/master/sc2/game_state.py#L82) through `self.state.<variable_name>`, 

- all [the game info variables](https://github.com/Dentosal/python-sc2/blob/master/sc2/game_info.py#L126) through `self._game_info.<variable_name>`,

- your units [`self.units`](https://github.com/Dentosal/python-sc2/blob/master/sc2/units.py) and visible enemy units [`self.known_enemy_units`](https://github.com/Dentosal/python-sc2/blob/master/sc2/units.py) which contain units of type [Unit](https://github.com/Dentosal/python-sc2/blob/master/sc2/unit.py). Both have properties and functions available.


# Why does my bot behave differently realtime=True vs realtime=False?
realtime=False will wait for your bot step to finish before confirming the next game loop.

realtime=True on the other hand will not wait and will strictly continue. So your bot will only have a fixed amount of time to complete its step before the next game loop is called, regardless if your bot step is finished or not. That means calculating placement locations using `self.find_placement()` or `self.expand_now()` may result in a delayed action.
