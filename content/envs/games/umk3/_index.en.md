---
date: 2016-04-09T16:50:16+02:00
title: Ultimate Mortal Kombat 3
weight: 40
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/umk3.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
</figure>

### Index

<div style="font-size:1.125rem;">

- <a href="./#game-specific-info">Game Specific Info</a>
- <a href="./#specific-episode-settings">Specific Episode Settings</a>
- <a href="./#action-spaces">Action Spaces</a>
- <a href="./#observation-space">Observation Space</a>
  - <a href="./#global">Global</a>
  - <a href="./#player-specific">Player Specific</a>

</div>

### Game Specific Info

|                                                                                                                          |                                                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <strong><span style="color:#5B5B60;">Game ID</span></strong>                                                             | `umk3`                                                                                                                                                                                  |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>                                                   | `umk3r10.zip`<br><strong><span style="color:#FF0000;">Rename the rom from umk3r10.zip to umk3.zip</span></strong>                                                                       |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>                                                     | `f48216ad82f78cb86e9c07d2507be347f904f4b5ae354a85ae7c34d969d265af`                                                                                                                      |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>                                                     | `ULTIMATE MORTAL KOMBAT 3 (CLONE)`, `ultimate-mortal-kombat-3-clone`, `109574`, `wowroms`                                                                                               |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>                                      | 254px&#160;X&#160;500px&#160;X&#160;3                                                                                                                                                   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions</span></strong> | 9, 7 (7)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-6): (No-Attack, High Punch, High Kick, Low Kick, Low Punch, Run, Block) |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>                                            | 5                                                                                                                                                                                       |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>                                   | 26 (22)                                                                                                                                                                                 |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>                                               | 1                                                                                                                                                                                       |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>                                          | 8 (Tower 1), 9 (Tower 2), 10 (Tower 3), 11 (Tower 4)                                                                                                                                    |

### Specific Episode Settings

#### Game Settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>                                                                                                                          |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `difficulty`                                             | `None` U `int`                                                     | `None`                                                                     | [1, 5]                                                                                                                                                                                    |

##### Additional Game Settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `tower`                                                  | `int`                                                     | 3                                                                     | [1, 4]                                                           | Selects the tower to play in (1P mode only)                      |

#### Player Settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>                                                                                                                          |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `characters*`                                             | `None` U `str` U `tuple` of maximum three `str`                   | `None`                                                              | Kitana, Reptile, Kano, Sektor, Kabal, Sonya, Mileena, Sindel, Sheeva, Jax, Ermac, Stryker, Shang Tsung, Nightwolf, Sub-Zero-2, Cyrax, Liu Kang, Jade, Sub-Zero, Kung Lao, Smoke, Skorpion |
| `outfits*`                                           | `int`                                                     | 1                                                                     | [1, 1]                                                                                                                                                                                    |

{{% notice note %}}
*: must be provided as tuples of two elements (for `agent_0` and `agent_1` respectively) when using the environments in two players mode.
{{% /notice %}}

### Action Spaces

| <strong><span style="color:#5B5B60;">Type</span></strong>                                                          | <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
| ------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>            | 9 (moves) + 7 (attacks) - 1 (no-op counted twice) = 15                         |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | 9 (moves) X 7 (attacks) = 63                                                         |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/umk3Data.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of Ultimate Mortal Kombat 3 RAM states</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                     | <strong><span style="color:#5B5B60;">Value</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------------- |
| `frame`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0,&#160;255] X [254&#160;X&#160;500&#160;X&#160;3]        | Latest game frame (RGB pixel screen)                             |
| `stage`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [1, 11]                                                    | Current stage of the game                                        |
| `timer`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0, 100]                                                           | Round time remaining                                        |

#### Player specific

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                                        | <strong><span style="color:#5B5B60;">Value</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                                                                                                                                                                                                                                                                                |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `side`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1]                                                | Side of the stage where the player is<br>0: Left, 1: Right                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `wins`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;2]                                                | Number of rounds won by the player                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `character`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;25]                                               | Index of character in use<br>0: Kitana, 1: Reptile, 2: Kano, 3: Sektor, 4: Kabal, 5: Sonya, 6: Mileena, 7: Sindel, 8: Sheeva, 9: Jax, 10: Ermac, 11: Stryker, 12: Shang Tsung, 13: Nightwolf, 14: Sub-Zero-2, 15: Cyrax, 16: Liu Kang, 17: Jade, 18: Sub-Zero, 19: Kung Lao, 20: Smoke, 21: Skorpion, 22: Human Smoke, 23: Noob Saibot, 24: Motaro", 25: Shao Kahn                                                                                                              |
| `health`                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;166]                                              | Health bar value                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `aggressor_bar`                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0, 48]                                                    | Aggressor bar value                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
