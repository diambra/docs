---
title: Environments
weight: 30
---

### Environment Settings

| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> |
|-------------|-------------| ------|------|------|
| <strong><span style="color:#5B5B60;">Game Selection</span></strong> | `gameId` | `string` | - | - |
| <strong><span style="color:#5B5B60;">Path to ROMs Folder</span></strong> | `romsPath` | `string` | - | - |
| <strong><span style="color:#5B5B60;">Player Side Selection</span></strong> | `player` | `string` | `Random` | 1P Mode: P1 (left), P2 (right), Random (50% P1, 50% P2)<br>2P Mode: P1P2 |
| <strong><span style="color:#5B5B60;">Environment Rendering</span></strong> | `render` | `bool` | `True` | True / False |
| <strong><span style="color:#5B5B60;">60 FPS Lock</span></strong> | `lockFps` | `bool` | `True` | True / False |
| <strong><span style="color:#5B5B60;">Game Sound</span></strong> | `sound` | `bool` | settings["lockFps"] && settings["render"] | True / False  |
| <strong><span style="color:#5B5B60;">Environment/Emulator Steps Ratio</span></strong> | `stepRatio` | `int` | 6 | [1, 6] |
| <strong><span style="color:#5B5B60;">Headless Mode (For Server-Side Executions)</span></strong> | `headless` | `bool` | `False` | True / False |
| <strong><span style="color:#5B5B60;">Game Continue Logic (1P Mode Only)</span></strong> | `conitnueGame` | `double` | `0.0` | (-inf, 1.0]<br>`[0.0, 1.0]`: probability of continuing game at game over<br>`int(abs(-inf, -1.0])`: number of continues at game over before episode to be considered done |
| <strong><span style="color:#5B5B60;">Show Game Final</span></strong> | `showFinal` | `bool` | `True` | True / False |

### Game Specific Info

|  <strong><span style="color:#5B5B60;">Parameter</span></strong>  | <strong><span style="color:#5B5B60;">Description</span></strong>  |
|-------------|-------------|
| <strong><span style="color:#5B5B60;">Game ID</span></strong>   | String identifying the game  |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>   | Name of the original game ROM to be downloaded (if renaming is needed, it will be indicated)      |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>  | ROM file checksum used to validate it |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>   | List of keywords that can be used to find the correct ROM file   |
| <strong><span style="color:#5B5B60;">Game Resolution (H X W X C)</span></strong>  | Game frame resolution   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong>  | Number of moves and attack actions and their descrption   |
| <strong><span style="color:#5B5B60;">Max Difficulty</span></strong>  | Maximum difficulty level available   |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>  | Number of characters featured in the game, and those that can actually be selected  |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>  | Maximum number of different outfits available per each character |
| <strong><span style="color:#5B5B60;">Max Stage</span></strong>  | Maximum number of stages for the single player mode   |

### Game Specific Settings

| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>|
|-------------|-------------| ------|------|
| <strong><span style="color:#5B5B60;">Difficulty (1P Mode Only)</span></strong>   | `difficulty`       | `int`| [1, 4]|
| <strong><span style="color:#5B5B60;">Characters List</span></strong>   | `characters`| `string`       | Kasumi, Zack, Hayabusa, Bayman, Lei-Fang, Raidou, Gen-Fu, Tina, Bass, Jann-Lee, Ayane |
| <strong><span style="color:#5B5B60;">Characters Outfits</span></strong>   | `charOutfits`| `int`      | [1, 4] |

### Action Space

| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Values</span></strong>| <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
|-------------|-------------| ------|-------| 
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Not Active  | `discrete`, `False` | 9 (moves) + 4 (attacks) - 1 (no-action counted twice) = 12 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Active  | `discrete`, `True` | 9 (moves) + 8 (attacks) - 1 (no-action counted twice) = 16 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Not Active  | `multiDiscrete`, `False` | 9 (moves) X 4 (attacks) = 36 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Active  | `multiDiscrete`, `True` | 9 (moves) X 8 (attacks) = 72 |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="/images/envs/doappData.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of DOA++ additional observations</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Frame</span></strong>   | `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> |[0,&#160;255] X [480&#160;X&#160;512&#160;X&#160;3] | Last game frame  (RGB pixel screen)|
| <strong><span style="color:#5B5B60;">Stage (1P Mode Only)</span></strong>   | `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0, 8]| Current stage of the game |

#### Player specific

To access the observation elements described in the following table, key(s) `P1` (1P and 2P Modes) and/or `P2` (2P Mode only) needs to be used as shown in the following snippet:

```python
ownSideVar = observation["P1"]["ownSide"]
```

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Side</span></strong>   | `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| <strong><span style="color:#5B5B60;">Wins</span></strong>   | `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;2]| Number of rounds won by the player |
| <strong><span style="color:#5B5B60;">Selected Character&#160;#1</span></strong>   | `ownChar1`/`oppChar1`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;10] | Index of character in use (for games where only one character is selected, this values is the same as "Character in Use")<br>0: Kasumi, 1: Zack, 2: Hayabusa, 3: Bayman, 4: Lei-Fang, 5: Raidou, 6: Gen-Fu, 7: Tina, 8: Bass, 9: Jann-Lee, 10: Ayane|
| <strong><span style="color:#5B5B60;">Character in Use</span></strong>   | `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;10] | Index of character in use<br>0: Kasumi, 1: Zack, 2: Hayabusa, 3: Bayman, 4: Lei-Fang, 5: Raidou, 6: Gen-Fu, 7: Tina, 8: Bass, 9: Jann-Lee, 10: Ayane|
| <strong><span style="color:#5B5B60;">Health</span></strong>   | `ownHealth`/`oppHealth` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;208]| Health bar value |
| <strong><span style="color:#5B5B60;">Actions-Move</span></strong>   | `actions`+`move`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;8] | Index of last move action performed (no-move, left, left+up, up, etc.)|
| <strong><span style="color:#5B5B60;">Actions-Attack</span></strong>   | `actions`+`attack`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;7] or [0,&#160;3]| Index of last attack action performed (no-attack, hold, punch, etc.) with, respectively, attack buttons combination active or not|

