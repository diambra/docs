---
date: 2016-04-09T16:50:16+02:00
title: Street Fighter III 3rd Strike
weight: 20
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/sfiii3n.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
</figure>

### Index

<div style="font-size:1.125rem;">

- <a href="./#game-specific-info">Game Specific Info</a>
- <a href="./#specific-variable-settings">Specific Variable Settings</a>
- <a href="./#action-spaces">Action Spaces</a>
- <a href="./#observation-space">Observation Space</a>
  - <a href="./#global">Global</a>
  - <a href="./#player-specific">Player Specific</a>

</div>

### Game Specific Info

|                                                                                                                          |                                                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <strong><span style="color:#5B5B60;">Game ID</span></strong>                                                             | `sfiii3n`                                                                                                                                                                                                                                                                   |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>                                                   | `sfiii3n.zip`                                                                                                                                                                                                                                                               |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>                                                     | `7239b5eb005488db22ace477501c574e9420c0ab70aeeb0795dfeb474284d416`                                                                                                                                                                                                          |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>                                                     | `STREET FIGHTER III 3RD STRIKE: FIGHT FOR THE FUTUR [JAPAN] (CLONE)`, `street-fighter-iii-3rd-strike-fight-for-the-futur-japan-clone`, `106255`, `wowroms`                                                                                                                  |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>                                      | 224px&#160;X&#160;384px&#160;X&#160;3                                                                                                                                                                                                                                       |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions</span></strong> | 9, 10 (7)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-9): No-Attack, Low Punch, Medium Punch, High Punch, Low Kick, Medium Kick, High Kick, Low Punch+Low Kick, Medium Punch+Medium Kick, High Punch+High Kick |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>                                            | 8                                                                                                                                                                                                                                                                           |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>                                   | 20 (19)                                                                                                                                                                                                                                                                     |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>                                               | 7                                                                                                                                                                                                                                                                           |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>                                          | 10                                                                                                                                                                                                                                                                          |

### Specific Variable Settings
#### Game Settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>                                                        |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `difficulty`                                             | `str` U `int`                                                     | `Random`                                                                     | `Random` U [1, 8]                                                                                                                  |

#### Player Settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>                                                        |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `characters*`                                             | `str` or `tuple` of maximum three `str`                   | `Random`                                                              | Alex, Twelve, Hugo, Sean, Makoto, Elena, Ibuki, Chun-Li, Dudley, Necro, Q, Oro, Urien, Remy, Ryu, Gouki, Yun, Yang, Ken |
| `outfits*`                                           | `int`                                                     | 1                                                                     | [1, 7]                                                                                                                  |

##### Additional Player Settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>      |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------- |
| `super_art*`                                              | `str` U `int`                                                     | `Random`                                                                     | `Random` U [1, 3]                                                           | Selects the type of super move.<br>1-2-3: Super move 1-2-3 |

{{% notice note %}}
*: must be provided as tuples of two elements (for `agent_0` and `agent_1` respectively) when using the environments in two players mode.
{{% /notice %}}

### Action Spaces

| <strong><span style="color:#5B5B60;">Type</span></strong>                                                          | <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
| ------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>            | 9 (moves) + 10 (attacks) - 1 (no-op counted twice) = 18                         |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | 9 (moves) X 10 (attacks) = 90                                                         |                                                    |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/sfiii3nData.png" style="margin-bottom:20px;">
</figure>

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/sfiii3nData2.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of Street Fighter III RAM states</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                     | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `frame`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0,&#160;255] X [224&#160;X&#160;384&#160;X&#160;3]              | Latest game frame (RGB pixel screen)                             |
| `stage`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [1, 10]                                                          | Current stage of the game                                        |
| `timer`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0, 100]                                                           | Round time remaining                                        |

#### Player specific

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                                        | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                                                                                                                                              |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `own_side`/`opp_side`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1]                                                      | Side of the stage where the player is<br>0: Left, 1: Right                                                                                                                                                                                                                                                                                    |
| `own_wins`/`opp_wins`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;2]                                                      | Number of rounds won by the player                                                                                                                                                                                                                                                                                                            |
| `own_char`/`opp_char`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;19]                                                     | Index of character in use<br>0: Alex, 1: Twelve, 2: Hugo, 3: Sean, 4: Makoto, 5: Elena, 6: Ibuki, 7: Chun-Li, 8: Dudley, 9: Necro, 10: Q, 11: Oro, 12: Urien, 13: Remy, 14: Ryu, 15: Gouki, 16: Yun, 17: Yang, 18: Ken, 19: Gill                                                                                                              |
| `own_health`/`opp_health`                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [-1,&#160;160]                                                   | Health bar value                                                                                                                                                                                                                                                                                                                              |
| `action_move`                                         | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;8]                                                      | Index of latest move action performed (no-move, left, left+up, up, etc.)                                                                                                                                                                                                                                                                      |
| `action_attack`                                       | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;10]                                      | Index of latest attack action performed (no-attack, low punch, medium punch, etc.)                                                                                                                                                                                              |
| `own_stun_bar`/`opp_stun_bar`                                | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;72]                                                     | Stun bar value                                                                                                                                                                                                                                                                                                                                |
| `own_stunned`/`opp_stunned`                                | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1]                                                      | Stunned flag                                                                                                                                                                                                                                                                                                                                  |
| `own_super_bar`/`opp_super_bar`                              | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;128]                                                    | Super bar value                                                                                                                                                                                                                                                                                                                               |
| `own_super_type`/`opp_super_type`                            | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;2]                                                      | Selected type of super move<br>0-1-2: Super Type 1-2-3                                                                                                                                                                                                                                                                                        |
| `own_super_count`/`opp_super_count`                          | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;3]                                                      | Count of activated super moves                                                                                                                                                                                                                                                                                                                |
| `own_super_max`/`opp_super_max`                              | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [1,&#160;3]                                                      | Maximum number of activated super moves                                                                                                                                                                                                                                                                                                       |
