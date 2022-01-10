---
date: 2016-04-09T16:50:16+02:00
title: Street Fighter III 3rd Strike
weight: 20
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="./images/envs/sfiii3n.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
</figure>

### Index                                                                       
                                                                                
- <a href="/envs/games/sfiii3n/#game-specific-info" style="font-size:20px;">Game Specific Info</a>
- <a href="/envs/games/sfiii3n/#game-specific-settings" style="font-size:20px;">Game Specific Settings</a>
- <a href="/envs/games/sfiii3n/#action-space-settings" style="font-size:20px;">Action Space Settings</a>
- <a href="/envs/games/sfiii3n/#observation-space" style="font-size:20px;">Observation Space</a>
    - <a href="/envs/games/sfiii3n/#global" style="font-size:20px;">Global</a>                
    - <a href="/envs/games/sfiii3n/#player-specific" style="font-size:20px;">Player Specific</a>

### Game Specific Info

|       |  |
|-------------|-------------|
| <strong><span style="color:#5B5B60;">Game ID</span></strong>   | `sfiii3n`       |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>   | `sfiii3n.zip`       |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>  | `7239b5eb005488db22ace477501c574e9420c0ab70aeeb0795dfeb474284d416`        |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>   | `STREET FIGHTER III 3RD STRIKE: FIGHT FOR THE FUTUR [JAPAN] (CLONE)`, `street-fighter-iii-3rd-strike-fight-for-the-futur-japan-clone`, `106255`, `wowroms`     |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>  | 224px&#160;X&#160;384px&#160;X&#160;3   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong>  | 9, 10 (7)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-9): (No-Attack, Low Punch, Medium Punch, High Punch, Low Kick, Medium Kick, High Kick), Low Punch+Low Kick, Medium Punch+Medium Kick, High Punch+High Kick   |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>  | 8   |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>  | 20 (19)   |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>  | 7  |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>  | 10   |

### Game Specific Settings

| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>| <strong><span style="color:#5B5B60;">Value Range</span></strong>|
|-------------|-------------| ------|------| ----|
| <strong><span style="color:#5B5B60;">Difficulty (1P Mode Only)</span></strong>   | `difficulty`       | `int`| 3 |[1, 8]|
| <strong><span style="color:#5B5B60;">Characters List</span></strong>   | `characters`| `string`       | [[`Random`],&#160;[`Random`]] | Alex, Twelve, Hugo, Sean, Makoto, Elena, Ibuki, Chun-Li, Dudley, Necro, Q, Oro, Urien, Remy, Ryu, Gouki, Yun, Yang, Ken |
| <strong><span style="color:#5B5B60;">Characters Outfits</span></strong>   | `charOutfits`| `int`      | [2, 2] | [1, 7] |
| <strong><span style="color:#5B5B60;">Super Art</span></strong>   | `superArt`| `int`      | [0, 0] | [0, 3] |

### Action Space Settings

| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Keys</span></strong> | <strong><span style="color:#5B5B60;">Values</span></strong>| <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
|-------------|-------------| ------|-------| ------- |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Not Active  | `actionSpace`,  `attackButCombination` | `discrete`, `False` | 9 (moves) + 7 (attacks) - 1 (no-action counted twice) = 15 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Active  | `actionSpace`,  `attackButCombination` | `discrete`, `True` | 9 (moves) + 10 (attacks) - 1 (no-action counted twice) = 18 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Not Active  | `actionSpace`,  `attackButCombination` | `multiDiscrete`, `False` | 9 (moves) X 7 (attacks) = 63 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Active  | `actionSpace`,  `attackButCombination` | `multiDiscrete`, `True` | 9 (moves) X 10 (attacks) = 90 |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="./images/envs/sfiii3nData.png" style="margin-bottom:20px;">
</figure>

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="./images/envs/sfiii3nData2.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of Street Fighter III additional observations</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Frame</span></strong>   | `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> |[0,&#160;255] X [224&#160;X&#160;384&#160;X&#160;3] | Last game frame  (RGB pixel screen)|
| <strong><span style="color:#5B5B60;">Stage (1P Mode Only)</span></strong>   | `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [1, 10]| Current stage of the game |

#### Player specific

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Side</span></strong>   | `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| <strong><span style="color:#5B5B60;">Wins</span></strong>   | `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;2]| Number of rounds won by the player |
| <strong><span style="color:#5B5B60;">Selected Character&#160;#1</span></strong>   | `ownChar1`/`oppChar1`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;19] | Index of first character selected (since in this game only one character is selected, these values are the same as "Character in Use")<br>0: Alex, 1: Twelve, 2: Hugo, 3: Sean, 4: Makoto, 5: Elena, 6: Ibuki, 7: Chun-Li, 8: Dudley, 9: Necro, 10: Q, 11: Oro, 12: Urien, 13: Remy, 14: Ryu, 15: Gouki, 16: Yun, 17: Yang, 18: Ken, 19: Gill|
| <strong><span style="color:#5B5B60;">Character in Use</span></strong>   | `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;19] | Index of character in use<br>0: Alex, 1: Twelve, 2: Hugo, 3: Sean, 4: Makoto, 5: Elena, 6: Ibuki, 7: Chun-Li, 8: Dudley, 9: Necro, 10: Q, 11: Oro, 12: Urien, 13: Remy, 14: Ryu, 15: Gouki, 16: Yun, 17: Yang, 18: Ken, 19: Gill|
| <strong><span style="color:#5B5B60;">Health</span></strong>   | `ownHealth`/`oppHealth` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [-1,&#160;160]| Health bar value |
| <strong><span style="color:#5B5B60;">Actions-Move</span></strong>   | `actions`+`move`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;8] | Index of last move action performed (no-move, left, left+up, up, etc.)|
| <strong><span style="color:#5B5B60;">Actions-Attack</span></strong>   | `actions`+`attack`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;10] or [0,&#160;7]| Index of last attack action performed (no-attack, low punch, medium punch, etc.) with, respectively, attack buttons combination active or not|
