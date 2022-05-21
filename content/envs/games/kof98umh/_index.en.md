---
date: 2016-04-09T16:50:16+02:00
title: The King of Fighers '98 UMH
weight: 60
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/kof98umh.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
</figure>

### Index                                                                       
                                                                                
<div style="font-size:1.125rem;">

- <a href="./#game-specific-info">Game Specific Info</a>
- <a href="./#game-specific-settings">Game Specific Settings</a>
- <a href="./#action-spaces">Action Spaces</a>
- <a href="./#observation-space">Observation Space</a>
    - <a href="./#global">Global</a>                
    - <a href="./#player-specific">Player Specific</a>

</div>

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

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>| <strong><span style="color:#5B5B60;">Value Range</span></strong>|
|-------------| ------|------| ----|
| `difficulty`       | `int`| 3 |[1, 8]|
| `characters`| `string`       | [[`Random`], [`Random`]]| Kyo, Benimaru, Daimon, Terry, Andy, Joe, Ryo, Robert, Yuri, Leona, Ralf, Clark, Athena, Kensou, Chin, Chizuru, Mai, King, Kim, Chang, Choi, Yashiro, Shermie, Chris, Yamazaki, Mary, Billy, Iori, Mature, Vice, Heidern, Takuma, Saisyu, Heavy-D!, Lucky, Brian, Eiji, Kasumi, Shingo, Rugal, Geese, Krauser, Mr.Big |
| `charOutfits`| `int`      | [2, 2] | [1, 4] |

##### Extended game settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>| <strong><span style="color:#5B5B60;">Value Range</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong>
|-------------| ------|------| ----|------|
| `fightingStyle`       | `int`| [0, 0] |[0, 3]| Selects the fighting style.<br>0: Random, 1: Advanced, 2: Extra, 3: Ultimate
| `ultimateStyle`       | `int`| [[0, 0, 0], [0, 0, 0]] |[0, 2]&#160;X&#160;[0, 2]&#160;X&#160;[0, 2]| Selects details about Ultimate Fighting Style for Dash, Evade and Bar features.<br>0: Random, 1: Advanced, 2: Extra |

### Action Spaces

| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
| ------|-------| ------- |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Not Active  | 9 (moves) + 5 (attacks) - 1 (no-action counted twice) = 13 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Active  | 9 (moves) + 9 (attacks) - 1 (no-action counted twice) = 17 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Not Active  | 9 (moves) X 5 (attacks) = 45 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Active  | 9 (moves) X 9 (attacks) = 81 |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/kof98umhData.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of The King of Fighters '98 Ultimate Match additional observations</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------| ------|-------| --------------|
| `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> |[0,&#160;255] X [240&#160;X&#160;320&#160;X&#160;3] | Last game frame  (RGB pixel screen)|
| `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [1, 7]| Current stage of the game |

#### Player specific

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------| ------|-------| --------------|
| `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;3]| Number of rounds won by the player |
| `ownChar1`/`oppChar1`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;44] | Index of first character selected<br>0: Kyo, 1: Benimaru, 2: Daimon, 3: Terry, 4: Andy, 5: Joe, 6: Ryo, 7: Robert, 8: Yuri, 9: Leona, 10: Ralf, 11: Clark, 12: Athena, 13: Kensou, 14: Chin, 15: Chizuru, 16: Mai, 17: King, 18: Kim, 19: Chang, 20: Choi, 21: Yashiro, 22: Shermie, 23: Chris, 24: Yamazaki, 25: Mary, 26: Billy, 27: Iori, 28: Mature, 29: Vice, 30: Heidern, 31: Takuma, 32: Saisyu, 33: Heavy-D!, 34: Lucky, 35: Brian, 36: Eiji, 37: Kasumi, 38: Shingo, 39: Rugal, 40: Geese, 41: Krauser, 42: Mr.Big, 43: Goenitz, 44: Orochi|
| `ownChar2`/`oppChar2`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;44] | Index of second character selected<br>0: Kyo, 1: Benimaru, 2: Daimon, 3: Terry, 4: Andy, 5: Joe, 6: Ryo, 7: Robert, 8: Yuri, 9: Leona, 10: Ralf, 11: Clark, 12: Athena, 13: Kensou, 14: Chin, 15: Chizuru, 16: Mai, 17: King, 18: Kim, 19: Chang, 20: Choi, 21: Yashiro, 22: Shermie, 23: Chris, 24: Yamazaki, 25: Mary, 26: Billy, 27: Iori, 28: Mature, 29: Vice, 30: Heidern, 31: Takuma, 32: Saisyu, 33: Heavy-D!, 34: Lucky, 35: Brian, 36: Eiji, 37: Kasumi, 38: Shingo, 39: Rugal, 40: Geese, 41: Krauser, 42: Mr.Big, 43: Goenitz, 44: Orochi|
| `ownChar3`/`oppChar3`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;44] | Index of third character selected<br>0: Kyo, 1: Benimaru, 2: Daimon, 3: Terry, 4: Andy, 5: Joe, 6: Ryo, 7: Robert, 8: Yuri, 9: Leona, 10: Ralf, 11: Clark, 12: Athena, 13: Kensou, 14: Chin, 15: Chizuru, 16: Mai, 17: King, 18: Kim, 19: Chang, 20: Choi, 21: Yashiro, 22: Shermie, 23: Chris, 24: Yamazaki, 25: Mary, 26: Billy, 27: Iori, 28: Mature, 29: Vice, 30: Heidern, 31: Takuma, 32: Saisyu, 33: Heavy-D!, 34: Lucky, 35: Brian, 36: Eiji, 37: Kasumi, 38: Shingo, 39: Rugal, 40: Geese, 41: Krauser, 42: Mr.Big, 43: Goenitz, 44: Orochi|
| `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;44] | Index of character in use<br>0: Kyo, 1: Benimaru, 2: Daimon, 3: Terry, 4: Andy, 5: Joe, 6: Ryo, 7: Robert, 8: Yuri, 9: Leona, 10: Ralf, 11: Clark, 12: Athena, 13: Kensou, 14: Chin, 15: Chizuru, 16: Mai, 17: King, 18: Kim, 19: Chang, 20: Choi, 21: Yashiro, 22: Shermie, 23: Chris, 24: Yamazaki, 25: Mary, 26: Billy, 27: Iori, 28: Mature, 29: Vice, 30: Heidern, 31: Takuma, 32: Saisyu, 33: Heavy-D!, 34: Lucky, 35: Brian, 36: Eiji, 37: Kasumi, 38: Shingo, 39: Rugal, 40: Geese, 41: Krauser, 42: Mr.Big, 43: Goenitz, 44: Orochi|
| `ownHealth`/`oppHealth` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [-1,&#160;119]| Health bar value |
| `actions`+`move`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;8] | Index of last move action performed (no-move, left, left+up, up, etc.)|
| `actions`+`attack`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;7] or [0,&#160;3]| Index of last attack action performed (no-attack, hold, punch, etc.) with, respectively, attack buttons combination active or not|
| `ownPowerBar`/`oppPowerBar` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;100]| Power bar value |
| `ownSpecialAttacks`/`oppSpecialAttacks` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;5]| Number of special attacks available |
| `ownActiveChar`/`oppActiveChar` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;2]| Index of the active character<br>0: first, 1: second, 2: third |
| `ownBarType`/`oppBarType`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;7] | Index of bar type<br>0: Advanced / Ultimate (Dash Advanced, Evade Advanced, Bar Advanced), 1: Extra / Ultimate (Dash Extra, Evade Extra, Bar Extra), 2: Ultimate (Dash Extra, Evade Advanced, Bar Advanced), 3: Ultimate (Dash Advanced, Evade Advanced, Bar Extra), 4: Ultimate (Dash Extra, Evade Advanced, Bar Extra), 5: Ultimate (Dash Advanced, Evade Extra, Bar Advanced), 6: Ultimate (Dash Extra, Evade Extra, Bar Advanced), 7: Ultimate (Dash Advanced, Evade Extra, Bar Extra) |
