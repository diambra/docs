---
date: 2016-04-09T16:50:16+02:00
title: Ultimate Mortal Kombat 3
weight: 40
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="/images/envs/umk3.jpg" style="margin-bottom:20px;">
</figure>

### Game Specific Info

|       |  |
|-------------|-------------|
| <strong><span style="color:#5B5B60;">Game ID</span></strong>   | `doapp`       |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>   | `doapp.zip`       |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>  | `d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e`        |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>   | `DEAD OR ALIVE ++ [JAPAN]`, `dead-or-alive-japan`, `80781`, `wowroms`       |
| <strong><span style="color:#5B5B60;">Game Resolution (H X W X C)</span></strong>  | 480px&#160;X&#160;512px&#160;X&#160;3   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong>  | 9, 8 (4)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-7): (No-Attack, Hold, Punch, Kick), Hold+Punch, Hold+Kick, Punch+Kick, Hold+Punch+Kick   |
| <strong><span style="color:#5B5B60;">Max Difficulty</span></strong>  | 4   |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>  | 11 (11)   |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>  | 4  |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>  | 8   |

### Game Specific Settings

| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>| <strong><span style="color:#5B5B60;">Value Range</span></strong>|
|-------------|-------------| ------|------| ----|
| <strong><span style="color:#5B5B60;">Difficulty (1P Mode Only)</span></strong>   | `difficulty`       | `int`| 3 |[1, 4]|
| <strong><span style="color:#5B5B60;">Characters List</span></strong>   | `characters`| `string`       | [[`Random`], [`Random`]]|Kasumi, Zack, Hayabusa, Bayman, Lei-Fang, Raidou, Gen-Fu, Tina, Bass, Jann-Lee, Ayane |
| <strong><span style="color:#5B5B60;">Characters Outfits</span></strong>   | `charOutfits`| `int`      | [4, 4] | [1, 4] |

### Action Space

| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Values</span></strong>| <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
|-------------|-------------| ------|-------| 
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Not Active  | `discrete`, `False` | 9 (moves) + 4 (attacks) - 1 (no-action counted twice) = 12 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Active  | `discrete`, `True` | 9 (moves) + 8 (attacks) - 1 (no-action counted twice) = 16 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Not Active  | `multiDiscrete`, `False` | 9 (moves) X 4 (attacks) = 36 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Active  | `multiDiscrete`, `True` | 9 (moves) X 8 (attacks) = 72 |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="/images/envs/umk3Data.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of Ultimate Mortal Kombat 3 additional observations</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Frame</span></strong>   | `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> |[0,&#160;255] X [480&#160;X&#160;512&#160;X&#160;3] | Last game frame  (RGB pixel screen)|
| <strong><span style="color:#5B5B60;">Stage (1P Mode Only)</span></strong>   | `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0, 8]| Current stage of the game |

#### Player specific

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Side</span></strong>   | `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| <strong><span style="color:#5B5B60;">Wins</span></strong>   | `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;2]| Number of rounds won by the player |
| <strong><span style="color:#5B5B60;">Selected Character&#160;#1</span></strong>   | `ownChar1`/`oppChar1`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;10] | Index of character in use (for games where only one character is selected, this values is the same as "Character in Use")<br>0: Kasumi, 1: Zack, 2: Hayabusa, 3: Bayman, 4: Lei-Fang, 5: Raidou, 6: Gen-Fu, 7: Tina, 8: Bass, 9: Jann-Lee, 10: Ayane|
| <strong><span style="color:#5B5B60;">Character in Use</span></strong>   | `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;10] | Index of character in use<br>0: Kasumi, 1: Zack, 2: Hayabusa, 3: Bayman, 4: Lei-Fang, 5: Raidou, 6: Gen-Fu, 7: Tina, 8: Bass, 9: Jann-Lee, 10: Ayane|
| <strong><span style="color:#5B5B60;">Health</span></strong>   | `ownHealth`/`oppHealth` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;208]| Health bar value |
| <strong><span style="color:#5B5B60;">Actions-Move</span></strong>   | `actions`+`move`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;8] | Index of last move action performed (no-move, left, left+up, up, etc.)|
| <strong><span style="color:#5B5B60;">Actions-Attack</span></strong>   | `actions`+`attack`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;7] or [0,&#160;3]| Index of last attack action performed (no-attack, hold, punch, etc.) with, respectively, attack buttons combination active or not|

