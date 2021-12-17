---
title: Utils
weight: 50
---

- <a href="/utils/#available-games">Available Games</a>
- <a href="/utils/#check-game-sha256-checksum">Check Game SHA256 Checksum</a>
- <a href="/utils/#environment-spaces-summary">Environment Spaces Summary</a>
- <a href="/utils/#show-gym-observation">Show Gym Observation</a>
- <a href="/utils/#show-wrapped-observation">Show Wrapped Observation</a>

#### Available Games

```python
import diambraArena
diambraArena.availableGames(printOut=True, details=True)
```

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

#### Check Game SHA256 Checksum

```python
import diambraArena
diambraArena.checkGameSha256(path="path/to/specific/rom/file.zip", gameId=None)
```

#### Environment Spaces Summary

```python
from diambraArena.gymUtils import envSpacesSummary
...
envSpacesSummary(env=environment)
```

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

#### Show Gym Observation

```python
from diambraArena.gymUtils import showGymObs
...
showGymObs(observation=obs, charList=charactersNames, waitKey=1, viz=True)
```

```txt
observation["frame"].shape: (480, 512, 3)
observation["stage"]: 1
observation["P1"]["ownChar1"]: Kasumi
observation["P1"]["oppChar1"]: Bayman
observation["P1"]["ownChar"]: Kasumi
observation["P1"]["oppChar"]: Bayman
observation["P1"]["ownHealth"]: 208
observation["P1"]["oppHealth"]: 208
observation["P1"]["ownSide"]: 0
observation["P1"]["oppSide"]: 1
observation["P1"]["ownWins"]: 0
observation["P1"]["oppWins"]: 0
observation["P1"]["actions"]: {'move': 0, 'attack': 3}
```

#### Show Wrapped Observation

```python
from diambraArena.gymUtils import showWrapObs
...
showWrapObs(observation=obs, nActionsStack=nActStack, charList=charactersNames, waitKey=1, viz=True)
```
```txt
observation["frame"].shape: (128, 128, 4)
observation["stage"]: 0.0
observation["P1"]["ownChar1"]: [0. 0. 0. 1. 0. 0. 0. 0. 0. 0. 0.] / Bayman
observation["P1"]["oppChar1"]: [1. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.] / Kasumi
observation["P1"]["ownChar"]: [0. 0. 0. 1. 0. 0. 0. 0. 0. 0. 0.] / Bayman
observation["P1"]["oppChar"]: [1. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.] / Kasumi
observation["P1"]["ownHealth"]: 1.0
observation["P1"]["oppHealth"]: 1.0
observation["P1"]["ownSide"]: 0
observation["P1"]["oppSide"]: 1
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
