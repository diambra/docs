---
date: 2016-04-09T16:50:16+02:00
title: Dead Or Alive ++
weight: 10
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="/images/envs/doapp.jpg" style="margin-bottom:20px;">
</figure>

### Game Specific Info

|       |  |
|-------------|-------------|
| <strong><span style="color:#5B5B60;">Game ID</span></strong>   | `doapp`       |
| <strong><span style="color:#5B5B60;">ROM Name</span></strong>   | `doapp.zip`       |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>  | `d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e`        |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>   | `DEAD OR ALIVE ++ [JAPAN]`, `dead-or-alive-japan`, `80781`, `wowroms`       |

### Game Specific Settings

| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>|
|-------------|-------------| ------|
| <strong><span style="color:#5B5B60;">Difficulty (1P Mode Only)</span></strong>   | `difficulty`       | [1, 4]|
| <strong><span style="color:#5B5B60;">Characters List</span></strong>   | `characters`       | Kasumi, Zack, Hayabusa, Bayman, Lei-Fang, Raidou, Gen-Fu, Tina, Bass, Jann-Lee, Ayane |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <a href="/images/envs/doappData.png" target="_blank"><img src="/images/envs/doappData.png" style="margin-bottom:20px;"></a>
  <figcaption align="middle">Some examples of additional observations</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Frame</span></strong>   | `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> |[0, 255] X [H X W X C] | Last game frame  (RGB pixel screen)|
| <strong><span style="color:#5B5B60;">Stage (1P Mode Only)</span></strong>   | `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0, 8]| Current stage of the game |

#### Player specific

Key: `P1` (1P and 2P Modes), `P2` (2P Mode only)

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Side</span></strong>   | `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| <strong><span style="color:#5B5B60;">Wins</span></strong>   | `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;2]| Number of rounds won by the player |
| <strong><span style="color:#5B5B60;">Character</span></strong>   | `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;10] | Index of character in use<br>0: Kasumi, 1: Zack, 2: Hayabusa, 3: Bayman, 4: Lei-Fang, 5: Raidou, 6: Gen-Fu, 7: Tina, 8: Bass, 9: Jann-Lee, 10: Ayane|
| <strong><span style="color:#5B5B60;">Health</span></strong>   | `ownHealth`/`oppHealth` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;208]| Health bar value |

&nbsp;&nbsp;&nbsp;&nbsp;<strong><span style="color:#5B5B60;">Actions</span></strong>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Last action performed.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Key: `actions`; Type: Dictionary; Value: `{"move": x, "attack": y}`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strong><span style="color:#5B5B60;">Move</span></strong>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Last move action performed, identifying all possible moves (left, left-up, up, etc.)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Key: `move`; Type: Discrete Categorical; Value: `(0, 9)`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strong><span style="color:#5B5B60;">Attack</span></strong>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Last attack action performed, identifying all possible moves (left, left-up, up, etc.)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Key: `attack`; Type: Discrete Categorical; Value: `(0, 7)` or `(0, 3)`, with or without attack buttons combinations active

### Action Space

#### Discrete

##### Attack Buttons Combination Off

##### Attack Buttons Combination On

#### MultiDiscrete

##### Attack Buttons Combination Off

##### Attack Buttons Combination On
