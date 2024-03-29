With everything installed in the [previous step](./Getting-started), it is time to create the first bot that will play against the games own AI.

# Strategy overview 

We will be using strategy called **Worker rush**, where we will select all our workers and attack to the enemy base. This can be done with any race, although Zerg is considered to be strongest, as their workers automatically regenerate health.

Worker rush is an extreme all-in build ([cheese](http://liquipedia.net/starcraft2/Cheese)). It usually loses the game, since the opponent will always have more workers than you, but incorrectly reacting to it can easily lose the game as well.

# Code, step by step

Create a new python-file to desired location and include the following code.

```python
# Import the library
import sc2
from sc2 import run_game, maps, Race, Difficulty
from sc2.player import Bot, Computer

# All bots inherit from sc2.BotAI
class WorkerRushBot(sc2.BotAI):

    # The on_step function is called for every game step
    # and all the actions should be called from within this function.
    async def on_step(self, iteration):

        if iteration == 0: # If this is the first frame

            for worker in self.workers:

                # Attack to the enemy base with this worker
                # (Assumes that there is only one possible starting location
                # for the opponent, which depends on the map)
                await self.do(worker.attack(self.enemy_start_locations[0]))      


# Parameters for starting the game.
# This will open a new window where the game is played.
run_game(maps.get("Abyssal Reef LE"), [
    Bot(Race.Zerg, WorkerRushBot()),
    Computer(Race.Protoss, Difficulty.Medium)
], realtime=True)
```

Now, by running the file, we can see the bot in action.

# On_step(self, iteration)
On_step is responsible for keeping your bot up to date with the game. It get's called on every game step, and your game logic should be called from inside the on_step function.

Notice that the on_step is asynchronous, so following practices of asynchronous programming is required.

# Issuing actions
Actions to be executed are passed through `self.do()` and should be passed for each unit individually. Here we have iterated over self.workers, which contains only the worker units.
