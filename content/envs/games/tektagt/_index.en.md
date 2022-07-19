---
date: 2016-04-09T16:50:16+02:00
title: Tekken Tag Tournament
weight: 30
---

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 400px;">
  <img src="../../../images/envs/tektagt.jpg" style="margin-bottom:20px; border-radius: 10px;"/>
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
| <strong><span style="color:#5B5B60;">Game ID</span></strong>   | `tektagt`       |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>   | `tektagtac.zip`<br><strong><span style="color:#FF0000;">Rename the rom from tektagtac.zip to tektagt.zip</span></strong>       |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>  | `57be777eae0ee9e1c035a64da4c0e7cb7112259ccebe64e7e97029ac7f01b168`        |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>   | `TEKKEN TAG TOURNAMENT [ASIA] (CLONE)`, `tekken-tag-tournament-asia-clone`, `108661`, `wowroms`      |
| <strong><span style="color:#5B5B60;">Game Resolution<br>(H X W X C)</span></strong>  | 240px&#160;X&#160;512px&#160;X&#160;3   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong>  | 9, 13 (6)<br>Moves (0-8): No-Move, Left, Left+Up, Up, Up+Right, Right, Right+Down, Down, Down+Left<br>Attacks (0-12): (No-Attack, Left Punch, Right Punch, Left Kick, Right Kick, Tag), Left Punch+Right Punch, Left Punch+Left Kick, Left Punch+Right Kick, Right Punch+Left Kick, Right Punch+Right Kick, Right Punch+Tag, Left Kick+Right Kick   |
| <strong><span style="color:#5B5B60;">Max Difficulty (1P Mode)</span></strong>  | 9   |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>  | 39 (38)   |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>  | 2  |
| <strong><span style="color:#5B5B60;">Number of Stages (1P Mode)</span></strong>  | 8   |

### Game Specific Settings

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>| <strong><span style="color:#5B5B60;">Value Range</span></strong>|
|-------------| ------|------| ----|
| `difficulty`       | `int`| 3 |[1, 9]|
| `characters`| `string`       | [[`Random`,&#160;`Random`],&#160;[`Random`,&#160;`Random`]] | Xiaoyu, Yoshimitsu, Nina, Law, Hwoarang, Eddy, Paul, King, Lei, Jin, Baek, Michelle, Armorking, Gunjack, Anna, Brian, Heihachi, Ganryu, Julia, Jun, Kunimitsu, Kazuya, Bruce, Kuma, Jack-Z, Lee, Wang, P.Jack, Devil, True Ogre, Ogre, Roger, Tetsujin, Panda, Tiger, Angel, Alex, Mokujin |
| `char_outfits`| `int`      | [2, 2] | [1, 2] |

### Action Spaces

| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
|-------------|-------------| ------|
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Not Active  | 9 (moves) + 6 (attacks) - 1 (no-action counted twice) = 14 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | Active  | 9 (moves) + 13 (attacks) - 1 (no-action counted twice) = 21 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Not Active  | 9 (moves) X 6 (attacks) = 54 |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a> | Active  | 9 (moves) X 13 (attacks) = 117 |

### Observation Space

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/tektagtData.png" style="margin-bottom:20px;">
</figure>

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../../../images/envs/tektagtData2.png" style="margin-bottom:20px;">
  <figcaption align="middle">Some examples of Tekken Tag Tournament additional observations</figcaption>
</figure>

#### Global

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------| ------|-------| --------------|
| `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> |[0,&#160;255] X [240&#160;X&#160;512&#160;X&#160;3] | Latest game frame  (RGB pixel screen)|
| `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [1, 8]| Current stage of the game |

#### Player specific

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------| ------|-------| --------------|
| `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;2]| Number of rounds won by the player |
| `ownChar1`/`oppChar1`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;38] | Index of first character selected<br>0: Xiaoyu, 1: Yoshimitsu, 2: Nina, 3: Law, 4: Hwoarang, 5: Eddy, 6: Paul, 7: King, 8: Lei, 9: Jin, 10: Baek, 11: Michelle, 12: Armorking, 13: Gunjack, 14: Anna, 15: Brian, 16: Heihachi, 17: Ganryu, 18: Julia, 19: Jun, 20: Kunimitsu, 21: Kazuya, 22: Bruce, 23: Kuma, 24: Jack-Z, 25: Lee, 26: Wang, 27: P.Jack, 28: Devil, 29: True Ogre, 30: Ogre, 31: Roger, 32: Tetsujin, 33: Panda, 34: Tiger, 35: Angel, 36: Alex, 37: Mokujin, 38: Unknown|
| `ownChar2`/`oppChar2`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;38] | Index of second character selected<br>0: Xiaoyu, 1: Yoshimitsu, 2: Nina, 3: Law, 4: Hwoarang, 5: Eddy, 6: Paul, 7: King, 8: Lei, 9: Jin, 10: Baek, 11: Michelle, 12: Armorking, 13: Gunjack, 14: Anna, 15: Brian, 16: Heihachi, 17: Ganryu, 18: Julia, 19: Jun, 20: Kunimitsu, 21: Kazuya, 22: Bruce, 23: Kuma, 24: Jack-Z, 25: Lee, 26: Wang, 27: P.Jack, 28: Devil, 29: True Ogre, 30: Ogre, 31: Roger, 32: Tetsujin, 33: Panda, 34: Tiger, 35: Angel, 36: Alex, 37: Mokujin, 38: Unknown|
| `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;38] | Index of character in use<br>0: Xiaoyu, 1: Yoshimitsu, 2: Nina, 3: Law, 4: Hwoarang, 5: Eddy, 6: Paul, 7: King, 8: Lei, 9: Jin, 10: Baek, 11: Michelle, 12: Armorking, 13: Gunjack, 14: Anna, 15: Brian, 16: Heihachi, 17: Ganryu, 18: Julia, 19: Jun, 20: Kunimitsu, 21: Kazuya, 22: Bruce, 23: Kuma, 24: Jack-Z, 25: Lee, 26: Wang, 27: P.Jack, 28: Devil, 29: True Ogre, 30: Ogre, 31: Roger, 32: Tetsujin, 33: Panda, 34: Tiger, 35: Angel, 36: Alex, 37: Mokujin, 38: Unknown|
| `ownHealth1`/`oppHealth1` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;182] | Health bar value for first character in use|
| `ownHealth2`/`oppHealth2` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;182] | Health bar value for second character in use|
| `actions`+`move`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;8] | Index of latest move action performed (no-move, left, left+up, up, etc.)|
| `actions`+`attack`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;12] or [0,&#160;5]| Index of latest attack action performed (no-attack, left punch, right punch, etc.) with, respectively, attack buttons combination active or not|
| `ownActiveChar`/`oppActiveChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Index of the active character<br>0: first, 1: second |
| `ownBarStatus`/`oppBarStatus`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;4]| Status of the background character health bar<br>0: reserve health bar almost filled, 1: small amount of health lost, recharging in progress, 2: large amount of health lost, recharging in progress, 3: rage mode on, combo attack ready, 4: no background character (final boss)|
