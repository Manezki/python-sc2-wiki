The purpose of this Getting Started section is to give you an understanding how to: make sure you are able to launch a game and run your bot. [Examples folder](https://github.com/Dentosal/python-sc2/tree/master/examples) of the repository contains more impactful examples.

# Installation 

Note that there are three parts for the installation: Python-sc2, SC II game-client and Maps.

### Python-sc2
Before we continue, make sure you have Python version of at least 3.6 available.

This can be checked on Linux and MacOs with terminal, using following command:
```
python3 --version
```

Now the python-sc2 package can be installed via pip:

```
pip3 install --user --upgrade sc2
```

### StarCraft II client and maps
### Game client
* #### Windows & MacOs
If you have not played StarCraft II before, the installation will require account creation and a download of ``Battle.net`` client. StarCraft II is free and can be downloaded through ``Battle.net``. To start the journey, head over to [Blizzard website](https://us.battle.net/account/sc2/starter-edition/) and grab the starter version.

* #### Linux
For Linux only headless version is available, and the binaries and installation instructions can be found [here](https://github.com/Blizzard/s2client-proto/blob/master/docs/linux.md#setup). Even though the binaries are headless - the games will produce replays that can be inspected after the game.

### Maps
Before running the bots is possible, some maps need to be manually installed. The map-packs in [Blizzard/sc2client-proto](https://github.com/Blizzard/s2client-proto#map-packs) are recommended. Don't be too picky, just download them all.

Download the zips and extract them to 'StarCraft II's Maps-folder ( password for the zips is: 'iagreetotheeula', referring to [here](https://github.com/Blizzard/s2client-proto#downloads) ).
If you did use default options when installing StarCraft II, you should find the Maps folder in:
* Windows: ``"C:/Program Files (x86)/StarCraft II/Maps"``
* MacOs: ``Applications/StarCraft II/Maps``
* Linux: ``~/StarCraftII/Maps``
The maps should be left into the folders that they are contained in.

# Building your first bot
With everything now installed, we are ready to try your [first bot](./Very-first-bot).