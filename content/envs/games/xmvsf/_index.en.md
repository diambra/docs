---
date: 2016-04-09T16:50:16+02:00
title: X-Men VS Street Fighter
weight: 80
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/xmvsf.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
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
| <strong><span style="color:#5B5B60;">Game ID</span></strong>                                                             | `xmvsf`                                                                                                                                                                                                |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong></strong>                                                   | `xmvsf.zip`<br><strong><span style="color:#FF0000;">Requires the QSound_HLE sound driver to be placed in the roms folder. Search keywords: "qsound_hle.zip", "dl-1425.bin"</span></strong>                                                   |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>                                                     | `833aa46af63a3ad87f69ce2bacd85a4445f35a50e3aff4f793f069b205b51c60`                                                                                                                                     |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>                                                     | `x-men vs street fighter`, `x-men-vs.-street-fighter-usa-961004`, `8769`, `wowroms`                                                                                                                                  |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>                                      | 224px&#160;X&#160;384px&#160;X&#160;3                                                                                                                                                                  |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions</span></strong> | 9, 18 (7)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-17): No-Attack, Weak Punch, Medium Punch, Strong Punch, Weak Kick, Medium Kick, Strong Kick, WP+MP, MP+SP, WP+SP, WK+MK, MK+SK, WK+SK, WP+WK, SP+SK, MP+WK, WP+MP+SP, WK+MK+SK |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>                                            | 8                                                                                                                                                                                                      |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>                                   | 19 (17)                                                                                                                                                                                                |
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
| `characters*`                                             | `None` U `str` U `tuple` of maximum three `str`                   | `None`                                                              | Akuma, Magneto, Juggernaut, Dhalsim, Mr. Bison, Sabretooth, Storm, Chun-Li, Zangief, Gambit, Rogue, Cammy, Charlie, Wolverine, Cyclops, Ryu, Ken |
`outfits*`                                           | `int`                                                     | 1                                                                     | [1, 2]                                                                                |

{{% notice note %}}
*: must be provided as tuples of two elements (for `agent_0` and `agent_1` respectively) when using the environments in two players mode.
{{% /notice %}}

### Action Spaces

| <strong><span style="color:#5B5B60;">Type</span></strong>                                                          | <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
| ------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>            | 9 (moves) + 18 (attacks) - 1 (no-op counted twice) = 26                         |
| <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | 9 (moves) X 18 (attacks) = 162                                                         |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/xmvsfData.jpg" style="margin-bottom:20px;">
</figure>
<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/xmvsfData2.jpg" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of X-Men VS Street Fighter RAM states</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                     | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `frame`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0,&#160;255] X [224&#160;X&#160;384&#160;X&#160;3]              | Latest game frame (RGB pixel screen)                             |
| `stage`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [1, 8]                                                           | Current stage of the game                                        |
| `timer`                                                  | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a> | [0, 99]                                                           | Round time remaining                                        |

#### Player specific

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong>                                                        | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                                                                  |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `side`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1]                                                      | Side of the stage where the player is<br>0: Left, 1: Right                                                                                                                                                                                                        |
| `wins`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;1]                                                      | Number of rounds won by the player                                                                                                                                                                                                                                |
| `character_1`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;18]                                                     | Index of first character slot<br>0: Akuma, 1: Magneto, 2: Juggernaut, 3: Dhalsim, 4: Mr. Bison, 5: Sabretooth, 6: Storm, 7: Chun-Li, 8: Zangief, 9: Gambit, 10: Rogue, 11: Cammy, 12: Charlie, 13: Wolverine, 14: Cyclops, 15: Ryu, 16: Ken, 17: Apocalypse, 18: Alpha Chun-Li                                                                                                               |
| `character_2`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;18]                                                     | Index of second character slot<br>0: Akuma, 1: Magneto, 2: Juggernaut, 3: Dhalsim, 4: Mr. Bison, 5: Sabretooth, 6: Storm, 7: Chun-Li, 8: Zangief, 9: Gambit, 10: Rogue, 11: Cammy, 12: Charlie, 13: Wolverine, 14: Cyclops, 15: Ryu, 16: Ken, 17: Apocalypse, 18: Alpha Chun-Li                                                                                                                |
| `character`                                      | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a>          | [0,&#160;18]                                                     | Index of character in use<br>0: Akuma, 1: Magneto, 2: Juggernaut, 3: Dhalsim, 4: Mr. Bison, 5: Sabretooth, 6: Storm, 7: Chun-Li, 8: Zangief, 9: Gambit, 10: Rogue, 11: Cammy, 12: Charlie, 13: Wolverine, 14: Cyclops, 15: Ryu, 16: Ken, 17: Apocalypse, 18: Alpha Chun-Li                                                                                                                |
| `health_1`                                 | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;144]                                                    | Health bar value for first character in use                                                                                                                                                                                                                                                  |
| `health_2`                                 | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;144]                                                    | Health bar value for second character in use                                                                                                                                                                                                                                                  |
| `active_character`                          | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1]                                                      | Index of the active character<br>1: first, 0: second                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `super_bar`                              | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;144]                                                    | Super bar value                                                                                                                                                                                                                                                                                                                               |
| `super_count`                          | <a href="https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/spaces/box.py" target="blank_">Box</a>                    | [0,&#160;3]                                                      | Count of activated super moves                                                                                                                                                                                                                                                                                                                |