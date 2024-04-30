---
date: 2016-04-09T16:50:16+02:00
title: Soul Calibur
weight: 90
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/soulclbr.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
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

|                                                                                                                          |                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <strong><span style="color:#5B5B60;">Game ID</span></strong>                                                             | `soulclbr`                                                                                                                                                                                                |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>                                                   | `soulclbr.zip`                                                                                                                                                                                            |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>                                                     | `a07a1a19995d582b56f2865783c5d7adb7acb9a6ad995a26fc7c4cfecd821817`                                                                                                                                     |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>                                                     | `soul calibur`, `soul-calibur`, `106959`, `wowroms`                                                                                                                                  |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>                                      | 240&#160;X&#160;512px&#160;X&#160;3                                                                                                                                                                  |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions</span></strong> | 9, 13 (5)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-12): No-Attack, Horizontal Attack, Vertical Attack, Kick, Guard, HA+VA, HA+K, VA+K, HA+G, VA+G, K+G, HA+VA+K, HA+VA+G |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>                                            | 5                                                                                                                                                                                                      |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>                                   | 18 (17)                                                                                                                                                                                                |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>                                               | 2                                                                                                                                                                                                      |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>                                          | 8                                                                                                                                                                                                      |

### Specific Episode Settings

#### Game Settings

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>                      |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| `difficulty`                                             | `None` U `int`                                                     | `None`                                                                     | [1, 8]                                                                                |

#### Player Settings

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>                      |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| `characters*`                                             | `None` U `str` U `tuple` of maximum three `str`                   | `None`                                                              | Xianghua, Yoshimitsu, Lizard Man, Siegfried, Rock, Seung Mina, Edge Master, Voldo, Ivy, Sophitia, Arthur, Kilik, Hwang, Maxi, Nightmare, Taki, Astaroth |
| `outfits*`                                           | `int`                                                     | 1                                                                     | [1, 2]                                                                                |

{{% notice note %}}
*: must be provided as tuples of two elements (for `agent_0` and `agent_1` respectively) when using the environments in two players mode.
{{% /notice %}}

### Action Spaces

| <strong><span style="color:#5B5B60;">Type</span></strong>                                                          | <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
| ------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>            | 9 (moves) + 13 (attacks) - 1 (no-op counted twice) = 21                         |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | 9 (moves) X 13 (attacks) = 117                                                         |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/soulclbrData.jpg" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of Soul Calibur RAM states</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                     | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `frame`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0,&#160;255] X [240&#160;X&#160;512&#160;X&#160;3]              | Latest game frame (RGB pixel screen)                             |
| `stage`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [1, 8]                                                           | Current stage of the game                                        |
| `timer`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0, 40]                                                           | Round time remaining                                        |

#### Player specific

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                                        | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                                                                  |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `side`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1]                                                      | Side of the stage where the player is<br>0: Left, 1: Right                                                                                                                                                                                                        |
| `wins`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;2]                                                      | Number of rounds won by the player                                                                                                                                                                                                                                |
| `character`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;17]                                                     | Index of character in use<br>0: Xianghua, 1: Yoshimitsu, 2: Lizard Man, 3: Siegfried, 4: Rock, 5: Seung Mina, 6: Edge Master, 7: Voldo, 8: Ivy, 9: Sophitia, 10: Arthur, 11: Kilik, 12: Hwang, 13: Maxi, 14: Nightmare, 15: Taki, 16:  Astaroth, 17: Inferno                                                                                                              |
| `health`                                 | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;240]                                                    | Health bar value                                                                                                                                                                                                                                                  |
