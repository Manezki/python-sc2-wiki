# Strategy overview

Select all starting workers and attack to the enemy base. This can be done with any race, although Zerg is considered to be strongest, as their workers automatically regenerate health.

Worker rush is an extreme all-in build ([cheese](http://liquipedia.net/starcraft2/Cheese)). It usually loses the game, since the opponent will always have more workers than you, but incorrectly reacting to it can easily lose the game as well.

# Code, step by step

```python
# Import the library
import sc2

# All bots inherit from sc2.BotAI
class WorkerRushBot(sc2.BotAI):

    # The on_step function is called for every game step
    # It is defined as async because it calls await functions
    # It takes current game state and current iteration
    async def on_step(self, state, iteration):

        if iteration == 0: # If this is the first frame

            for worker in self.workers:

                # Attack to the enemy base with this worker
                # (Assumes that there is only one possible starting location
                # for the opponent, which depends on the map)
                await self.do(worker.attack(self.enemy_start_locations[0]))      
```

If you want to run this against a built in AI:

```python
from sc2 import run_game, maps, Race, Difficulty
from sc2.player import Bot, Computer

run_game(maps.get("Abyssal Reef LE"), [
    Bot(Race.Zerg, WorkerRushBot()),
    Computer(Race.Protoss, Difficulty.Medium)
], realtime=True)
```


Or if you want to play against this yourself:

```python
from sc2 import run_game, maps, Race
from sc2.player import Bot, Human

run_game(maps.get("Abyssal Reef LE"), [
    Human(Race.Terran),
    Bot(Race.Zerg, WorkerRushBot())
], realtime=True)
```