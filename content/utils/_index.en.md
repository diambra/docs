---
title: Utils
weight: 50
---

### Index

<div style="font-size:1.125rem;">

- <a href="./#available-games">Available Games</a>
- <a href="./#roms-check">ROMs Check</a>
- <a href="./#environment-spaces-summary">Environment Spaces Summary</a>
- <a href="./#observation-inspection">Observation Inspection</a>
    - <a href="./#gym-observation">Gym Observation</a>
    - <a href="./#wrapped-observation">Wrapped Observation</a>
- <a href="./#gamepad-interface">Gamepad Interface</a>

</div>

DIAMBRA Arena comes with many different tools supporting development and debug. They provide different functionalities, all described below in the sections below where both code and output is reported. 

Source code can be found in the code repository, <a href="../wrappers/#reward-clipping" target="_blank">here ADD LINK</a>

### Available Games

Provides a list of available games and their most important details. It is executed as shown below:

```python
import diambraArena
diambraArena.availableGames(printOut=True, details=True)
```

Output will be similar to what follows:

```txt
 Title: Dead Or Alive ++ - GameId: doapp
   Difficulty levels: Min 1 - Max 4
   SHA256 sum: d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
   Original ROM name: doapp.zip
   Search keywords: ['DEAD OR ALIVE ++ [JAPAN]', 'dead-or-alive-japan', '80781', 'wowroms']
   Characters list: ['Kasumi', 'Zack', 'Hayabusa', 'Bayman', 'Lei-Fang', 'Raidou', 'Gen-Fu', 'Tina', 'Bass', 'Jann-Lee', 'Ayane']

 Title: Street Fighter III - GameId: sfiii3n
   Difficulty levels: Min 1 - Max 8
   SHA256 sum: 7239b5eb005488db22ace477501c574e9420c0ab70aeeb0795dfeb474284d416
   Original ROM name: sfiii3n.zip
   Search keywords: ['STREET FIGHTER III 3RD STRIKE: FIGHT FOR THE FUTUR [JAPAN] (CLONE)', 'street-fighter-iii-3rd-strike-fight-for-the-futur-japan-clone', '106255', 'wowroms']
   Characters list: ['Alex', 'Twelve', 'Hugo', 'Sean', 'Makoto', 'Elena', 'Ibuki', 'Chun-Li', 'Dudley', 'Necro', 'Q', 'Oro', 'Urien', 'Remy', 'Ryu', 'Gouki', 'Yun', 'Yang', 'Ken', 'Gill']

 Title: Tekken Tag Tournament - GameId: tektagt
   Difficulty levels: Min 1 - Max 9
   SHA256 sum: 57be777eae0ee9e1c035a64da4c0e7cb7112259ccebe64e7e97029ac7f01b168
   Original ROM name: tektagtac.zip
   Search keywords: ['TEKKEN TAG TOURNAMENT [ASIA] (CLONE)', 'tekken-tag-tournament-asia-clone', '108661', 'wowroms']
   Notes: Rename the rom from tektagtac.zip to tektagt.zip
   Characters list: ['Xiaoyu', 'Yoshimitsu', 'Nina', 'Law', 'Hwoarang', 'Eddy', 'Paul', 'King', 'Lei', 'Jin', 'Baek', 'Michelle', 'Armorking', 'Gunjack', 'Anna', 'Brian', 'Heihachi', 'Ganryu', 'Julia', 'Jun', 'Kunimitsu', 'Kazuya', 'Bruce', 'Kuma', 'Jack-Z', 'Lee', 'Wang', 'P.Jack', 'Devil', 'True Ogre', 'Ogre', 'Roger', 'Tetsujin', 'Panda', 'Tiger', 'Angel', 'Alex', 'Mokujin', 'Unknown']
...
```

### ROMs Check

Checks ROM SHA256 checksum to validate them.

#### Without `gameId` specification

If no `gameId` is specified, the ROM file provided as first argument, will be verified against all available games. It is executed as shown below:

```python
import diambraArena
diambraArena.checkGameSha256(path="path/to/specific/rom/doapp.zip", gameId=None)
```

Output will be similar to what follows:

```txt
Correct ROM file for Dead Or Alive ++, sha256 = d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
```

#### With `gameId` specification

If `gameId` is specified, the ROM file provided checksum will be compared with the one of the specified game identified by `gameId`. It is executed as shown below:

```python
import diambraArena
diambraArena.checkGameSha256(path="path/to/specific/rom/doapp.zip", gameId="umk3")
```
Output will be similar to what follows:

```txt
Expected  SHA256 Checksum: f48216ad82f78cb86e9c07d2507be347f904f4b5ae354a85ae7c34d969d265af
Retrieved SHA256 Checksum: d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
```

### Environment Spaces Summary

Prints out a summary of Environment's Observation and Action spaces showing nesting levels, keys, space types and low/high bounds where applicable. It is executed as shown below:

```python
from diambraArena.gymUtils import envSpacesSummary
...
envSpacesSummary(env=environment)
```
Output will be similar to what follows:

```txt
Observation space:

        observation_space["P1"]["actions"]["attack"]: Discrete(4)

        observation_space["P1"]["actions"]["move"]: Discrete(9)

    observation_space["P1"]["oppChar"]: Discrete(11)

    observation_space["P1"]["oppChar1"]: Discrete(11)

    observation_space["P1"]["oppHealth"]: Box()
        Space type = int32
        Space high bound = 208
        Space low bound = 0

    observation_space["P1"]["oppSide"]: Discrete(2)

    observation_space["P1"]["oppWins"]: Box()
        Space type = int32
        Space high bound = 2
        Space low bound = 0

    observation_space["P1"]["ownChar"]: Discrete(11)

    observation_space["P1"]["ownChar1"]: Discrete(11)

    observation_space["P1"]["ownHealth"]: Box()
        Space type = int32
        Space high bound = 208
        Space low bound = 0

    observation_space["P1"]["ownSide"]: Discrete(2)

    observation_space["P1"]["ownWins"]: Box()
        Space type = int32
        Space high bound = 2
        Space low bound = 0

observation_space["frame"]: Box(480, 512, 3)
        Space type = uint8
        Space high bound = [[[255 255 255]
  [255 255 255]
  [255 255 255]
  ...
  [255 255 255]
  [255 255 255]
  [255 255 255]]

 ...

 [[255 255 255]
  [255 255 255]
  [255 255 255]
  ...
  [255 255 255]
  [255 255 255]
  [255 255 255]]]
        Space low bound = [[[0 0 0]
  [0 0 0]
  [0 0 0]
  ...
  [0 0 0]
  [0 0 0]
  [0 0 0]]

 ...

 [[0 0 0]
  [0 0 0]
  [0 0 0]
  ...
  [0 0 0]
  [0 0 0]
  [0 0 0]]]

observation_space["stage"]: Box()
        Space type = int8
        Space high bound = 8
        Space low bound = 1

Action space:

action_space =  Discrete(12)
  Space type =  int64
    Space n =  12
```

### Observation Inspection

Prints out a detailed description of Environment's observation content, showing every level of it. The only element that is not printed in the terminal is the game frame that, when `viz` input argument is set to `True`, is shown is a dedicated graphical window. The `waitKey` parameter defines how this window behaves: when set equal to 0, it pauses waiting for the user to press a button, while if set different from zero, it waits the prescribed number of milliseconds before continuing.

There are two different methods, one to be used for the basic Gym Environment and the other one specific for the wrapped one.

#### Gym Observation

{{% notice tip %}}
Use of this functionality can be found in <a href="/gettingstarted/examples/singleplayerenv/">this</a> and <a href="/gettingstarted/examples/multiplayerenv/">this</a> examples.
{{% /notice %}}  

```python
from diambraArena.gymUtils import showGymObs
...
showGymObs(observation=obs, charList=charactersNames, waitKey=1, viz=True)
```
Output will be similar to what follows:

 - Frame visualization window:
<figure style="margin-right:auto; margin-left:auto;">
  <img src="../images/utils/gymObs.jpg" style="padding-left:40px;margin-bottom:1rem; margin-top:0px">
</figure> 

 - Terminal printout:
   ```txt
   observation["frame"].shape: (480, 512, 3)
   observation["stage"]: 1
   observation["P1"]["ownChar1"]: Kasumi
   observation["P1"]["oppChar1"]: Bayman
   observation["P1"]["ownChar"]: Kasumi
   observation["P1"]["oppChar"]: Bayman
   observation["P1"]["ownHealth"]: 66
   observation["P1"]["oppHealth"]: 184
   observation["P1"]["ownSide"]: 0
   observation["P1"]["oppSide"]: 1
   observation["P1"]["ownWins"]: 0
   observation["P1"]["oppWins"]: 0
   observation["P1"]["actions"]: {'move': 0, 'attack': 3}
   ```

#### Wrapped Observation

{{% notice tip %}}
Use of this functionality can be found in <a href="/gettingstarted/examples/wrappersoptions/">this</a>, <a href="/gettingstarted/examples/humanexperiencerecorder/">this</a> and <a href="/gettingstarted/examples/imitationlearning/">this</a> examples.
{{% /notice %}}  

{{% notice warning %}}
This functionality currently does not support all possible wrappers configurations but only a part of them. In particular, it assumes the observation normalization wrapper is active.
{{% /notice %}}  

```python
from diambraArena.gymUtils import showWrapObs
...
showWrapObs(observation=obs, nActionsStack=nActStack, charList=charactersNames, waitKey=1, viz=True)
```

Output will be similar to what follows:

- Frame stack visualization windows:
<figure style="margin-right:auto; margin-left:auto;">
  <img src="../images/utils/wrapObs.jpg" style="padding-left:40px;margin-bottom:1rem; margin-top:0px">
</figure> 

- Terminal printout:

  ```txt
  observation["frame"].shape: (128, 128, 4)
  observation["stage"]: 0.0
  observation["P1"]["ownChar1"]: [0. 0. 0. 1. 0. 0. 0. 0. 0. 0. 0.] / Bayman
  observation["P1"]["oppChar1"]: [1. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.] / Kasumi
  observation["P1"]["ownChar"]: [0. 0. 0. 1. 0. 0. 0. 0. 0. 0. 0.] / Bayman
  observation["P1"]["oppChar"]: [1. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.] / Kasumi
  observation["P1"]["ownHealth"]: 0.8173076923076923
  observation["P1"]["oppHealth"]: 0.8028846153846154
  observation["P1"]["ownSide"]: 1
  observation["P1"]["oppSide"]: 0
  observation["P1"]["ownWins"]: 0.0
  observation["P1"]["oppWins"]: 0.0
  observation["P1"]["actions"]["move"]:
  [[1 0 0 0 0 0 0 0 0]
   [1 0 0 0 0 0 0 0 0]
   [0 0 0 0 0 0 0 0 1]
   [1 0 0 0 0 0 0 0 0]
   [0 1 0 0 0 0 0 0 0]
   [0 1 0 0 0 0 0 0 0]
   [1 0 0 0 0 0 0 0 0]
   [1 0 0 0 0 0 0 0 0]
   [0 0 0 0 0 0 1 0 0]
   [0 0 0 0 0 0 1 0 0]
   [0 0 0 1 0 0 0 0 0]
   [0 0 0 0 0 0 0 0 1]]
  observation["P1"]["actions"]["attack"]:
  [[1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [0 0 1 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]]
  ```

### Gamepad Interface

{{% notice tip %}}
Use of this functionality can be found in <a href="/gettingstarted/examples/humanexperiencerecorder/">this</a> example.
{{% /notice %}}  

It allows to easily integrate a Game Pad to be used for experiments where human input is required, for example Human Expert Demonstration Collection or Competitive Human-Agent. The following code snippet shows a typical usage.

```python
import diambraArena
from diambraArena.utils.diambraGamepad import diambraGamepad
...
# GamePad(s) initialization
gamepad = diambraGamepad(env.actionList)
gamepad.start()
...
actions = gamepad.getActions()
```
