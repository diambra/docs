---
date: 2016-04-09T16:50:16+02:00
title: Samurai Showdown 5 Special
weight: 50
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/samsh5sp.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
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
| <strong><span style="color:#5B5B60;">Game ID</span></strong>   | `samsh5sp`       |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>   | `samsh5sp.zip`       |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>  | `adf33d8a02f3d900b4aa95e62fb21d9278fb920b179665b12a489bd39a6c103d`        |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>   | `SAMURAI SHODOWN V SPECIAL`, `samurai-shodown-v-special`, `100347`, `wowroms`  |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>  | 224px&#160;X&#160;320px&#160;X&#160;3   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong>  | 9, 11 (5)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-10): (No-Attack, Weak Slash, Medium Slash, Kick, Meditation), Weak Slash + Medium Slash (Strong Slash), Medium Slash + Kick (Surprise Attack), Weak Slash + Kick, Kick + Meditation, Weak Slash + Medium Slash + Kick (Rage), Medium Slash + Kick + Meditation   |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>  | 8  |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>  | 28 (28)   |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>  | 4  |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>  | 9   |

### Game Specific Settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>| <strong><span style="color:#5B5B60;">Value Range</span></strong>|
|-------------| ------|------| ----|
| `difficulty`       | `int`| 3 |[1, 8]|
| `characters`| `string`       | [[`Random`], [`Random`]]| Kyoshiro, Jubei, Hanzo, Enja, Amakusa, Suija, Galford, Charlotte, Kusare, Sogetsu, Gaira, Ukyo, Yoshitora, Gaoh, Haohmaru, Genjuro, Shizumaru, Kazuki, Tamtam, Rasetsumaru, Rimururu, Mina, Zankuro, Nakoruru, Rera, Yunfei, Basara, Mizuki |
| `charOutfits`| `int`      | [2, 2] | [1, 4] |

### Action Spaces

| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
|-------------|-------------| ------|
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Not Active  | 9 (moves) + 5 (attacks) - 1 (no-action counted twice) = 13 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Active  | 9 (moves) + 11 (attacks) - 1 (no-action counted twice) = 19 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Not Active  | 9 (moves) X 5 (attacks) = 45 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Active  | 9 (moves) X 11 (attacks) = 99 |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/samsh5spData.png" style="margin-bottom:20px;">
</figure>

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/samsh5spData2.png" style="margin-bottom:20px;">
</figure>

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/samsh5spData3.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of Samurai Showdown 5 Special additional observations</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------| ------|-------| --------------|
| `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> |[0,&#160;255] X [224&#160;X&#160;320&#160;X&#160;3] | Last game frame  (RGB pixel screen)|
| `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [1, 9]| Current stage of the game |

#### Player specific

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------| ------|-------| --------------|
| `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;2]| Number of rounds won by the player |
| `ownChar1`/`oppChar1`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;27] | Index of character in use (for games where only one character is selected, this values is the same as "Character in Use")<br>0: Kasumi, 1: Zack, 2: Hayabusa, 3: Bayman, 4: Lei-Fang, 5: Raidou, 6: Gen-Fu, 7: Tina, 8: Bass, 9: Jann-Lee, 10: Ayane|
| `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;27] | Index of character in use<br>0: Kasumi, 1: Zack, 2: Hayabusa, 3: Bayman, 4: Lei-Fang, 5: Raidou, 6: Gen-Fu, 7: Tina, 8: Bass, 9: Jann-Lee, 10: Ayane|
| `ownHealth`/`oppHealth` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;125]| Health bar value |
| `actions`+`move`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;8] | Index of last move action performed (no-move, left, left+up, up, etc.)|
| `actions`+`attack`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;7] or [0,&#160;3]| Index of last attack action performed (no-attack, hold, punch, etc.) with, respectively, attack buttons combination active or not|
| `ownRageOn`/`oppRageOn`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Rage on for the player<br>0: False, 1: True |
| `ownRageUsed`/`oppRageUsed`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Rage used by the player<br>0: False, 1: True |
| `ownWeaponLost`/`oppWeaponLost`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Weapon lost by the player<br>0: False, 1: True |
| `ownWeaponFight`/`oppWeaponFight`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Weapon fight condition triggered<br>0: False, 1: True |
| `ownRageBar`/`oppRageBar` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;164096]| Rage bar value |
| `ownWeaponBar`/`oppWeaponBar` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;120]| Weapon bar value |
| `ownPowerBar`/`oppPowerBar` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;64]| Power bar value |

