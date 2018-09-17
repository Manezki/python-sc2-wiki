# Should I use the [pysc2](https://github.com/deepmind/pysc2) or [python-sc2](https://github.com/Dentosal/python-sc2) library?
If you plan on making scripted hardcoded bots, python-sc2 is simpler in its syntax.

If you want to make a machine learning/deep learning bot, pysc2 is probably the better choice since it uses the UI and feature layers (spatial interfaces).

# How do I do \<something\>?
Feel free to take a look at [the protoss/terran/zerg examples](https://github.com/Dentosal/python-sc2/tree/master/examples).

# What do I have access to in the step() function?
You have access to:

- all the basic BotAI class properties and functions listed [here](https://github.com/Dentosal/python-sc2/blob/master/sc2/bot_ai.py) and [here](https://github.com/Dentosal/python-sc2/blob/master/sc2/bot_ai.py#L386) through `self.<property_name>` which contain basic functions that handle [macromanagement](https://liquipedia.net/starcraft2/Macro), 

- all [the game state variables](https://github.com/Dentosal/python-sc2/blob/master/sc2/game_state.py#L82) through `self.state.<variable_name>` which contain data that can change each step, 

- all [the game info variables](https://github.com/Dentosal/python-sc2/blob/master/sc2/game_info.py#L126) through `self._game_info.<variable_name>` which contain static map and player related data,

- all [the game data variables](https://github.com/Dentosal/python-sc2/blob/master/sc2/game_data.py#L25) through `self._game_data.<variable_name>` which contain static unit/ability/buff related data,

- your units [`self.units`](https://github.com/Dentosal/python-sc2/blob/master/sc2/units.py) and visible enemy units [`self.known_enemy_units`](https://github.com/Dentosal/python-sc2/blob/master/sc2/units.py) which is a list-like object and contain units of type [`Unit`](https://github.com/Dentosal/python-sc2/blob/master/sc2/unit.py). Both have properties and functions available.


# Why does my bot behave differently in realtime=True vs realtime=False?
realtime=False will wait for your bot step to finish before confirming the next game loop.

realtime=True on the other hand will not wait and will strictly continue. So your bot will only have a fixed amount of time to complete its step before the next game loop is called, regardless if your bot step is finished or not. That means calculating placement locations using `self.find_placement()` or `self.expand_now()` may result in a delayed action.

# This is all nice and good but where do I find better bot examples?
Bots that have been put on the [Bot ladder](http://sc2ai.net/) can be found here:
- [Bot by Hannessa](https://github.com/Hannessa/sc2-bots/tree/master/cannon-lover)
- [Bot by BuRny](https://github.com/BurnySc2/burny-bots-python-sc2/tree/master/CreepyBot)
- a collection of ladder bots (not restricted to python-sc2) https://github.com/m1ndgames/sc2ai-ladderbot-collection

Also the source code of bots from the ladder are sometimes publicly available.

# I think I am ready to put my bot on the ladder, how do I do that?

- Make a copy of the repo https://github.com/Hannessa/python-sc2-ladderbot
- Replace the example bot with your bot
- In `run.py` edit the following lines to match your bot's filename, name and race:
```python
from example_bot import ExampleBot
bot = Bot(Race.Random, ExampleBot())
```
- Add the `sc2` folder from `python-sc2` as a subfolder to make your bot immune to `python-sc2` updates that may break your bot
- Run the `run.py` to see if it works locally
- Update the zipped folder to http://sc2ai.net/ (after making an account)

# The pip version seems to be outdated, how do I install the version from github?
The command to install from github is
```
pip install pipenv
pip install --upgrade git+https://github.com/Dentosal/python-sc2.git
```
or alternatively
```
python3.6 -m pip install pipenv
python3.6 -m pip install --upgrade git+https://github.com/Dentosal/python-sc2.git
```