---
date: 2016-04-09T16:50:16+02:00
title: Dead Or Alive ++
weight: 10
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/doapp.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
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

|                                                                                                                          |                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <strong><span style="color:#5B5B60;">Game ID</span></strong>                                                             | `doapp`                                                                                                                                                                                                |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>                                                   | `doapp.zip`                                                                                                                                                                                            |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>                                                     | `d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e`                                                                                                                                     |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>                                                     | `DEAD OR ALIVE ++ [JAPAN]`, `dead-or-alive-japan`, `80781`, `wowroms`                                                                                                                                  |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>                                      | 480px&#160;X&#160;512px&#160;X&#160;3                                                                                                                                                                  |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong> | 9, 8 (4)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-7): (No-Attack, Hold, Punch, Kick), Hold+Punch, Hold+Kick, Punch+Kick, Hold+Punch+Kick |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>                                            | 4                                                                                                                                                                                                      |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>                                   | 11 (11)                                                                                                                                                                                                |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>                                               | 4                                                                                                                                                                                                      |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>                                          | 8                                                                                                                                                                                                      |

### Game Specific Settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>                      |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| `difficulty`                                             | `int`                                                     | 3                                                                     | [1, 4]                                                                                |
| `characters`                                             | `str` or `tuple` of maximum three `str`                   | `Random`                                                              | Kasumi, Zack, Hayabusa, Bayman, Lei-Fang, Raidou, Gen-Fu, Tina, Bass, Jann-Lee, Ayane |
| `outfits`                                           | `int`                                                     | 1                                                                     | [1, 4]                                                                                |

{{% notice note %}}
`characters` and `outfits` need to be provided as tuples of two elements (the first for P1 and the second for P2) when using this environment in two players mode.
{{% /notice %}}

### Action Spaces

| <strong><span style="color:#5B5B60;">Type</span></strong>                                                          | <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
| ------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>            | 9 (moves) + 8 (attacks) - 1 (no-op counted twice) = 16                         |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | 9 (moves) X 8 (attacks) = 72                                                         |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/doappData.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of Dead Or Alive ++ RAM states</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                     | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `frame`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0,&#160;255] X [480&#160;X&#160;512&#160;X&#160;3]              | Latest game frame (RGB pixel screen)                             |
| `stage`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [1, 8]                                                           | Current stage of the game                                        |
| `timer`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0, 40]                                                           | Round time remaining                                        |

#### Player specific

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                                        | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                                                                  |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `own_side`/`opp_side`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1]                                                      | Side of the stage where the player is<br>0: Left, 1: Right                                                                                                                                                                                                        |
| `own_wins`/`opp_wins`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;2]                                                      | Number of rounds won by the player                                                                                                                                                                                                                                |
| `own_char`/`opp_char`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;10]                                                     | Index of character in use<br>0: Kasumi, 1: Zack, 2: Hayabusa, 3: Bayman, 4: Lei-Fang, 5: Raidou, 6: Gen-Fu, 7: Tina, 8: Bass, 9: Jann-Lee, 10: Ayane                                                                                                              |
| `own_health`/`opp_health`                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;208]                                                    | Health bar value                                                                                                                                                                                                                                                  |
| `action_move`                                         | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;8]                                                      | Index of latest move action performed (no-move, left, left+up, up, etc.)                                                                                                                                                                                          |
| `action_attack`                                       | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;7]                                       | Index of latest attack action performed (no-attack, hold, punch, etc.)|
