---
title: Environments
weight: 30
---

### Index

- <a href="/envs/#environments-interaction-basics" style="font-size:20px;">Environment Interaction Basics</a>
- <a href="/envs/#environment-settings" style="font-size:20px;">Environment Settings</a>
- <a href="/envs/#game-specific-info" style="font-size:20px;">Game Specific Info</a>
- <a href="/envs/#game-specific-settings" style="font-size:20px;">Game Specific Settings</a>
- <a href="/envs/#action-spaces-settings" style="font-size:20px;">Action Space(s) Settings</a>
- <a href="/envs/#observation-space" style="font-size:20px;">Observation Space</a>
    - <a href="/envs/#global" style="font-size:20px;">Global</a>
    - <a href="/envs/#player-specific" style="font-size:20px;">Player Specific</a>
- <a href="/envs/#reward-function" style="font-size:20px;">Reward Function</a>

### Environment Interaction Basics

<div style="font-size:20px;">
DIAMBRA Arena Environments usage follows the standard RL interaction framework: the agent sends an action to the environment, which process it and performs a transition accordingly, from the starting state to the new state, returning the observation and the reward to the agent to close the interaction loop. The figure below shows this typical interaction scheme and data flow.

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 40%;">
  <img src="/images/envs/basicUsage.png" style="margin-bottom:20px;">
  <figcaption align="middle">Scheme of Agent-Environment Interaction</figcaption>
</figure>

The shortest snippet for a complete basic execution of an environment consists of just a few lines of code, and is presented in the code block below:
</div>


```python {linenos=inline}
import diambraArena 

# Environment settings
settings = {}
settings["gameId"] = "doapp"
settings["romsPath"] = "/path/to/roms/"

env = diambraArena.make("MyEnv", settings)

observation = env.reset()

while True:
    actions = env.action_space.sample()

    observation, reward, done, info = env.step(actions)

    if done:
        observation = env.reset()
        break

env.close()
```

<div style="font-size:20px;">

Line by line, one finds:
- 1 Package import
- 3-7 Keywords argument dictionary definition, featuring the two mandatory parameters: Game ID (in this case "doapp" for Dead Or Alive ++) and local path to game ROMs (with a placeholder string that needs to be substituted with the correct ROMs path)
- 8 Environment creation
- 10 Environment reset
- 12 Interaction loop start
- 13 Random action sampling
- 15 Environment stepping (action sent, observation and reward retrieved)
- 17 Episode done condition check
- 18 Environment reset
- 19 Interaction loop exit after episode end
- 21 Environment close

</div>

### Environment Settings

<div style="font-size:20px;">

All environments share a numerous set of options allowing to handle many different aspects, controlled by key-value pairs in a Python dictionary. The following table summarizes and describes the general, game-independent, settings, while the game-specific ones are presented in the game dedicated pages and, for those shared among all games, in the table contained in the <a href="/envs/#game-specific-settings">Game Specific Settings</a> section below.
</div>

| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> |
|-------------|-------------| ------|------|------|
| <strong><span style="color:#5B5B60;">Game Selection\*</span></strong> | `gameId` | `string` | - | - |
| <strong><span style="color:#5B5B60;">Path to ROMs Folder\*</span></strong> | `romsPath` | `string` | - | - |
| <strong><span style="color:#5B5B60;">Player Side Selection</span></strong> | `player` | `string` | `Random` | 1P Mode: P1 (left), P2 (right), Random (50% P1, 50% P2)<br>2P Mode: P1P2 |
| <strong><span style="color:#5B5B60;">Environment Rendering</span></strong> | `render` | `bool` | `True` | True / False |
| <strong><span style="color:#5B5B60;">60 FPS Lock</span></strong> | `lockFps` | `bool` | `True` | True / False |
| <strong><span style="color:#5B5B60;">Game Sound</span></strong> | `sound` | `bool` | settings["lockFps"] && settings["render"] | True / False  |
| <strong><span style="color:#5B5B60;">Environment/Emulator Steps Ratio</span></strong> | `stepRatio` | `int` | 6 | [1, 6] |
| <strong><span style="color:#5B5B60;">Headless Mode (For Server-Side Executions)</span></strong> | `headless` | `bool` | `False` | True / False |
| <strong><span style="color:#5B5B60;">Game Continue Logic (1P Mode Only)</span></strong> | `conitnueGame` | `double` | `0.0` | (-inf, 1.0]<br>`[0.0, 1.0]`: probability of continuing game at game over<br>`int(abs(-inf, -1.0])`: number of continues at game over before episode to be considered done |
| <strong><span style="color:#5B5B60;">Show Game Final</span></strong> | `showFinal` | `bool` | `True` | True / False |

\*: Mandatory

### Game Specific Info

<div style="font-size:20px;">                                                   

Game specific info provide useful details about it. They are reported in every game dedicated page, and summarized and described in the table below.
                                                                                
</div> 

|  <strong><span style="color:#5B5B60;">Parameter</span></strong>  | <strong><span style="color:#5B5B60;">Description</span></strong>  |
|-------------|-------------|
| <strong><span style="color:#5B5B60;">Game ID</span></strong>   | String identifying the game  |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>   | Name of the original game ROM to be downloaded (if renaming is needed, it is indicated)      |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>  | ROM file checksum used to validate it |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>   | List of keywords that can be used to find the correct ROM file   |
| <strong><span style="color:#5B5B60;">Game Resolution (H X W X C)</span></strong>  | Game frame resolution   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong>  | Number of moves and attack actions and their description   |
| <strong><span style="color:#5B5B60;">Max Difficulty</span></strong>  | Maximum difficulty level available   |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>  | Number of characters featured in the game, and those that can actually be selected  |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>  | Maximum number of different outfits available per each character |
| <strong><span style="color:#5B5B60;">Max Stage</span></strong>  | Maximum number of stages for the single player mode   |

### Game Specific Settings

<div style="font-size:20px;">                                                   

Environment settings that depend on the specific game and that are shared among all of them are reported in the table below. Each game dedicated page where additional ones (if present) are reported.
                                                                                
</div> 


| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> |
|-------------|-------------| ------|------|-----|
| <strong><span style="color:#5B5B60;">Difficulty (1P Mode Only)</span></strong>   | `difficulty`       | `int`| Default value if not specified | Min and Max values allowed for the specific game|
| <strong><span style="color:#5B5B60;">Characters List</span></strong>   | `characters`| `string`       | Default characters if not specified (for both P1 and P2) | List of characters games that can be selected for the specific game |
| <strong><span style="color:#5B5B60;">Characters Outfits</span></strong>   | `charOutfits`| `int`      | Default values if not specified (for both P1 and P2) | Min and Max values allowed for the specific game |

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;width: 40%">
  <img src="/images/envs/outfits.png" style="margin-bottom:20px;">
  <figcaption align="middle">Example of Dead or Alive ++ available outfits for Kasumi</figcaption>
</figure>

### Action Space(s) Settings

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;width: 40%;">
  <img src="/images/envs/actionSpaces.png" style="margin-bottom:20px;">
  <figcaption align="middle">Example of Dead Or Alive ++ Action Spaces</figcaption>
</figure>


| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Keys</span></strong> | <strong><span style="color:#5B5B60;">Values</span></strong>| <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
|-------------|-------------| ------|-------| ------- |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> / <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a>  | Active / Not Active  |`actionSpace`,  `attackButCombination` | `discrete`/`multiDiscrete`, `True`/`False` | Total number of actions available, divided in move and attack actions |

### Observation Space

<div style="font-size:20px;"> 

Environment observations are composed by two main elements: a visual one (the game frame) and an aggregation of quantitative values (like stage number, health values, etc.) called Additional Observations. Both of them are exposed by the environment through an observation space of type <a href="https://github.com/openai/gym/tree/master/gym/spaces/" target="blank_">`gym.spaces.Dict`</a>. It consists of global elements and player specific ones, they are presented and described in the tables below. In addition, next figure shows an example of Dead Or Alive ++ observation where some of the Additional Observations are highlighted, superimposed on the game frame.

Each game extends the set presented here with its specific one, described in the game dedicated page.

</div>

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="/images/envs/doappData.png" style="margin-bottom:20px;">
  <figcaption align="middle">An example of Dead Or Alive ++ additional observations</figcaption>
</figure>

#### Global

<div style="font-size:20px;"> 

</div>

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

### Reward Function


