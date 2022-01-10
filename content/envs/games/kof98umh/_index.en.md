---
date: 2016-04-09T16:50:16+02:00
title: The King of Fighers '98 UMH
weight: 60
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/kof98umh.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
</figure>

### Index                                                                       
                                                                                
- <a href="./#game-specific-info" style="font-size:20px;">Game Specific Info</a>
- <a href="./#game-specific-settings" style="font-size:20px;">Game Specific Settings</a>
- <a href="./#action-space-settings" style="font-size:20px;">Action Space Settings</a>
- <a href="./#observation-space" style="font-size:20px;">Observation Space</a>
    - <a href="./#global" style="font-size:20px;">Global</a>                
    - <a href="./#player-specific" style="font-size:20px;">Player Specific</a>

### Game Specific Info

|       |  |
|-------------|-------------|
| <strong><span style="color:#5B5B60;">Game ID</span></strong>   | `kof98umh`       |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>   | `kof98umh.zip`       |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>  | `beb7bdea87137832f5f6d731fd1abd0350c0cd6b6b2d57cab2bedbac24fe8d0a`        |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>   | `The King Of Fighters '98: Ultimate Match HERO`, `kof98umh`, `allmyroms`      |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>  | 240px&#160;X&#160;320px&#160;X&#160;3   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong>  | 9, 9 (5)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-8): (No-Attack, Weak Punch, Weak Kick, Strong Punch, Strong Kick), Weak Punch + Weak Kick, Strong Punch + Strong Kick, Weak Punch + Weak Kick + Strong Punch + Strong Kick, Weak Punch + Weak Kick + Strong Punch   |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>  | 8   |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>  | 45 (43)   |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>  | 4  |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>  | 7   |

### Game Specific Settings

| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>| <strong><span style="color:#5B5B60;">Value Range</span></strong>|
|-------------|-------------| ------|------| ----|
| <strong><span style="color:#5B5B60;">Difficulty (1P Mode Only)</span></strong>   | `difficulty`       | `int`| 3 |[1, 8]|
| <strong><span style="color:#5B5B60;">Fighting Style</span></strong>   | `fightingStyle`       | `int`| [0, 0] |[0, 3]|
| <strong><span style="color:#5B5B60;">Ultimate Fighting Style Configuration</span></strong>   | `ultimateStyle`       | `int`| [[0, 0, 0], [0, 0, 0]] |[0, 1]&#160;X&#160;[0, 1]&#160;X&#160;[0, 1]|
| <strong><span style="color:#5B5B60;">Characters List</span></strong>   | `characters`| `string`       | [[`Random`], [`Random`]]| Kyo, Benimaru, Daimon, Terry, Andy, Joe, Ryo, Robert, Yuri, Leona, Ralf, Clark, Athena, Kensou, Chin, Chizuru, Mai, King, Kim, Chang, Choi, Yashiro, Shermie, Chris, Yamazaki, Mary, Billy, Iori, Mature, Vice, Heidern, Takuma, Saisyu, Heavy-D!, Lucky, Brian, Eiji, Kasumi, Shingo, Rugal, Geese, Krauser, Mr.Big |
| <strong><span style="color:#5B5B60;">Characters Outfits</span></strong>   | `charOutfits`| `int`      | [2, 2] | [1, 4] |

### Action Space Settings

| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Keys</span></strong> | <strong><span style="color:#5B5B60;">Values</span></strong>| <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
|-------------|-------------| ------|-------| ------- |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Not Active  | `actionSpace`,  `attackButCombination` | `discrete`, `False` | 9 (moves) + 5 (attacks) - 1 (no-action counted twice) = 13 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Active  | `actionSpace`,  `attackButCombination` | `discrete`, `True` | 9 (moves) + 9 (attacks) - 1 (no-action counted twice) = 17 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Not Active  | `actionSpace`,  `attackButCombination` | `multiDiscrete`, `False` | 9 (moves) X 5 (attacks) = 45 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Active  | `actionSpace`,  `attackButCombination` | `multiDiscrete`, `True` | 9 (moves) X 9 (attacks) = 81 |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/kof98umhData.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of The King of Fighters '98 Ultimate Match additional observations</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Frame</span></strong>   | `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> |[0,&#160;255] X [240&#160;X&#160;320&#160;X&#160;3] | Last game frame  (RGB pixel screen)|
| <strong><span style="color:#5B5B60;">Stage (1P Mode Only)</span></strong>   | `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [1, 7]| Current stage of the game |

#### Player specific

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Side</span></strong>   | `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| <strong><span style="color:#5B5B60;">Wins</span></strong>   | `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;3]| Number of rounds won by the player |
| <strong><span style="color:#5B5B60;">Selected Character&#160;#1</span></strong>   | `ownChar1`/`oppChar1`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;44] | Index of first selected character<br>0: Kyo, 1: Benimaru, 2: Daimon, 3: Terry, 4: Andy, 5: Joe, 6: Ryo, 7: Robert, 8: Yuri, 9: Leona, 10: Ralf, 11: Clark, 12: Athena, 13: Kensou, 14: Chin, 15: Chizuru, 16: Mai, 17: King, 18: Kim, 19: Chang, 20: Choi, 21: Yashiro, 22: Shermie, 23: Chris, 24: Yamazaki, 25: Mary, 26: Billy, 27: Iori, 28: Mature, 29: Vice, 30: Heidern, 31: Takuma, 32: Saisyu, 33: Heavy-D!, 34: Lucky, 35: Brian, 36: Eiji, 37: Kasumi, 38: Shingo, 39: Rugal, 40: Geese, 41: Krauser, 42: Mr.Big, 43: Goenitz, 44: Orochi|
| <strong><span style="color:#5B5B60;">Selected Character&#160;#2</span></strong>   | `ownChar2`/`oppChar2`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;44] | Index of second selected character<br>0: Kyo, 1: Benimaru, 2: Daimon, 3: Terry, 4: Andy, 5: Joe, 6: Ryo, 7: Robert, 8: Yuri, 9: Leona, 10: Ralf, 11: Clark, 12: Athena, 13: Kensou, 14: Chin, 15: Chizuru, 16: Mai, 17: King, 18: Kim, 19: Chang, 20: Choi, 21: Yashiro, 22: Shermie, 23: Chris, 24: Yamazaki, 25: Mary, 26: Billy, 27: Iori, 28: Mature, 29: Vice, 30: Heidern, 31: Takuma, 32: Saisyu, 33: Heavy-D!, 34: Lucky, 35: Brian, 36: Eiji, 37: Kasumi, 38: Shingo, 39: Rugal, 40: Geese, 41: Krauser, 42: Mr.Big, 43: Goenitz, 44: Orochi|
| <strong><span style="color:#5B5B60;">Selected Character&#160;#3</span></strong>   | `ownChar3`/`oppChar3`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;44] | Index of third selected character<br>0: Kyo, 1: Benimaru, 2: Daimon, 3: Terry, 4: Andy, 5: Joe, 6: Ryo, 7: Robert, 8: Yuri, 9: Leona, 10: Ralf, 11: Clark, 12: Athena, 13: Kensou, 14: Chin, 15: Chizuru, 16: Mai, 17: King, 18: Kim, 19: Chang, 20: Choi, 21: Yashiro, 22: Shermie, 23: Chris, 24: Yamazaki, 25: Mary, 26: Billy, 27: Iori, 28: Mature, 29: Vice, 30: Heidern, 31: Takuma, 32: Saisyu, 33: Heavy-D!, 34: Lucky, 35: Brian, 36: Eiji, 37: Kasumi, 38: Shingo, 39: Rugal, 40: Geese, 41: Krauser, 42: Mr.Big, 43: Goenitz, 44: Orochi|
| <strong><span style="color:#5B5B60;">Character in Use</span></strong>   | `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;44] | Index of character in use<br>0: Kyo, 1: Benimaru, 2: Daimon, 3: Terry, 4: Andy, 5: Joe, 6: Ryo, 7: Robert, 8: Yuri, 9: Leona, 10: Ralf, 11: Clark, 12: Athena, 13: Kensou, 14: Chin, 15: Chizuru, 16: Mai, 17: King, 18: Kim, 19: Chang, 20: Choi, 21: Yashiro, 22: Shermie, 23: Chris, 24: Yamazaki, 25: Mary, 26: Billy, 27: Iori, 28: Mature, 29: Vice, 30: Heidern, 31: Takuma, 32: Saisyu, 33: Heavy-D!, 34: Lucky, 35: Brian, 36: Eiji, 37: Kasumi, 38: Shingo, 39: Rugal, 40: Geese, 41: Krauser, 42: Mr.Big, 43: Goenitz, 44: Orochi|
| <strong><span style="color:#5B5B60;">Health</span></strong>   | `ownHealth`/`oppHealth` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [-1,&#160;119]| Health bar value |
| <strong><span style="color:#5B5B60;">Actions-Move</span></strong>   | `actions`+`move`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;8] | Index of last move action performed (no-move, left, left+up, up, etc.)|
| <strong><span style="color:#5B5B60;">Actions-Attack</span></strong>   | `actions`+`attack`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;7] or [0,&#160;3]| Index of last attack action performed (no-attack, hold, punch, etc.) with, respectively, attack buttons combination active or not|
| <strong><span style="color:#5B5B60;">Power Bar</span></strong>   | `ownPowerBar`/`oppPowerBar` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;100]| Power bar value |
| <strong><span style="color:#5B5B60;">Special Attacks</span></strong>   | `ownSpecialAttacks`/`oppSpecialAttacks` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;5]| Number of special attacks available |
| <strong><span style="color:#5B5B60;">Active Character</span></strong>   | `ownActiveChar`/`oppActiveChar` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;2]| Index of the active character<br>0: first, 1: second, 2: third |
| <strong><span style="color:#5B5B60;">Bar Type</span></strong>   | `ownBarType`/`oppBarType`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;7] | Index of bar type<br>0: Advanced / Ultimate (Dash Advanced, Evade Advanced, Bar Advanced), 1: Extra / Ultimate (Dash Extra, Evade Extra, Bar Extra), 2: Ultimate (Dash Extra, Evade Advanced, Bar Advanced), 3: Ultimate (Dash Advanced, Evade Advanced, Bar Extra), 4: Ultimate (Dash Extra, Evade Advanced, Bar Extra), 5: Ultimate (Dash Advanced, Evade Extra, Bar Advanced), 6: Ultimate (Dash Extra, Evade Extra, Bar Advanced), 7: Ultimate (Dash Advanced, Evade Extra, Bar Extra) |
