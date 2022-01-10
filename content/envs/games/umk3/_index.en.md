---
date: 2016-04-09T16:50:16+02:00
title: Ultimate Mortal Kombat 3
weight: 40
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/umk3.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
</figure>

### Index                                                                       
                                                                                
- <a href="/envs/games/umk3/#game-specific-info" style="font-size:20px;">Game Specific Info</a>
- <a href="/envs/games/umk3/#game-specific-settings" style="font-size:20px;">Game Specific Settings</a>
- <a href="/envs/games/umk3/#action-space-settings" style="font-size:20px;">Action Space Settings</a>
- <a href="/envs/games/umk3/#observation-space" style="font-size:20px;">Observation Space</a>
    - <a href="/envs/games/umk3/#global" style="font-size:20px;">Global</a>                
    - <a href="/envs/games/umk3/#player-specific" style="font-size:20px;">Player Specific</a>

### Game Specific Info

|       |  |
|-------------|-------------|
| <strong><span style="color:#5B5B60;">Game ID</span></strong>   | `umk3`       |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>   | `umk3r10.zip`<br><strong><span style="color:#FF0000;">Rename the rom from umk3r10.zip to umk3.zip</span></strong>        |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>  | `f48216ad82f78cb86e9c07d2507be347f904f4b5ae354a85ae7c34d969d265af`        |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>   | `ULTIMATE MORTAL KOMBAT 3 (CLONE)`, `ultimate-mortal-kombat-3-clone`, `109574`, `wowroms`       |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>  | 254px&#160;X&#160;500px&#160;X&#160;3   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong>  | 9, 7 (7)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-6): (No-Attack, High Punch, High Kick, Low Kick, Low Punch, Run, Block)   |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>  | 5   |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>  | 26 (22)   |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>  | 2  |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>  | 8 (Tower 1), 9 (Tower 2), 10 (Tower 3), 11 (Tower 4)  |

### Game Specific Settings

| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>| <strong><span style="color:#5B5B60;">Value Range</span></strong>|
|-------------|-------------| ------|------| ----|
| <strong><span style="color:#5B5B60;">Difficulty (1P Mode Only)</span></strong>   | `difficulty`       | `int`| 3 |[1, 5]|
| <strong><span style="color:#5B5B60;">Tower (1P Mode Only)</span></strong>   | `tower`       | `int`| 3 |[1, 4]|
| <strong><span style="color:#5B5B60;">Characters List</span></strong>   | `characters`| `string`       | [[`Random`], [`Random`]]| Kitana, Reptile, Kano, Sektor, Kabal, Sonya, Mileena, Sindel, Sheeva, Jax, Ermac, Stryker, Shang Tsung, Nightwolf, Sub-Zero-2, Cyrax, Liu Kang, Jade, Sub-Zero, Kung Lao, Smoke, Skorpion |
| <strong><span style="color:#5B5B60;">Characters Outfits</span></strong>   | `charOutfits`| `int`      | [2, 2] | [1, 2] |

### Action Space Settings

| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Keys</span></strong> | <strong><span style="color:#5B5B60;">Values</span></strong>| <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
|-------------|-------------| ------|-------| ------- |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Not Active  |`actionSpace`,  `attackButCombination` |`discrete`, `False` | 9 (moves) + 7 (attacks) - 1 (no-action counted twice) = 15 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Active  |`actionSpace`,  `attackButCombination` | `discrete`, `True` | 9 (moves) + 7 (attacks) - 1 (no-action counted twice) = 15 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Not Active  |`actionSpace`,  `attackButCombination` | `multiDiscrete`, `False` | 9 (moves) X 7 (attacks) = 63 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Active  |`actionSpace`,  `attackButCombination` | `multiDiscrete`, `True` | 9 (moves) X 7 (attacks) = 63 |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/umk3Data.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of Ultimate Mortal Kombat 3 additional observations</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Frame</span></strong>   | `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> |[0,&#160;255] X [254&#160;X&#160;500&#160;X&#160;3] | Last game frame  (RGB pixel screen)|
| <strong><span style="color:#5B5B60;">Stage (1P Mode Only)</span></strong>   | `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [1, 11]| Current stage of the game |

#### Player specific

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Side</span></strong>   | `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| <strong><span style="color:#5B5B60;">Wins</span></strong>   | `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;2]| Number of rounds won by the player |
| <strong><span style="color:#5B5B60;">Selected Character&#160;#1</span></strong>   | `ownChar1`/`oppChar1`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;25] | Index of character in use (for games where only one character is selected, this values is the same as "Character in Use")<br>0: Kitana, 1: Reptile, 2: Kano, 3: Sektor, 4: Kabal, 5: Sonya, 6: Mileena, 7: Sindel, 8: Sheeva, 9: Jax, 10: Ermac, 11: Stryker, 12: Shang Tsung, 13: Nightwolf, 14: Sub-Zero-2, 15: Cyrax, 16: Liu Kang, 17: Jade, 18: Sub-Zero, 19: Kung Lao, 20: Smoke, 21: Skorpion, 22: Human Smoke, 23: Noob Saibot, 24: Motaro", 25: Shao Kahn|
| <strong><span style="color:#5B5B60;">Character in Use</span></strong>   | `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;25] | Index of character in use<br>0: Kitana, 1: Reptile, 2: Kano, 3: Sektor, 4: Kabal, 5: Sonya, 6: Mileena, 7: Sindel, 8: Sheeva, 9: Jax, 10: Ermac, 11: Stryker, 12: Shang Tsung, 13: Nightwolf, 14: Sub-Zero-2, 15: Cyrax, 16: Liu Kang, 17: Jade, 18: Sub-Zero, 19: Kung Lao, 20: Smoke, 21: Skorpion, 22: Human Smoke, 23: Noob Saibot, 24: Motaro", 25: Shao Kahn|
| <strong><span style="color:#5B5B60;">Health</span></strong>   | `ownHealth`/`oppHealth` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;166]| Health bar value |
| <strong><span style="color:#5B5B60;">Actions-Move</span></strong>   | `actions`+`move`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;8] | Index of last move action performed (no-move, left, left+up, up, etc.)|
| <strong><span style="color:#5B5B60;">Actions-Attack</span></strong>   | `actions`+`attack`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;6]| Index of last attack action performed (no-attack, high punch, high kick, etc.)|
| <strong><span style="color:#5B5B60;">Aggressor Bar</span></strong>   | `ownAggressorBar`/`oppAggressorBar` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0, 48]| Aggressor bar value |

