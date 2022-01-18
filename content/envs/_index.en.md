---
title: Environments
weight: 30
math: true
---

### Index

<div style="font-size:1.125rem;">

- <a href="./#overview">Overview</a>
- <a href="./#interaction-basics">Interaction Basics</a>
- <a href="./#settings">Settings</a>
- <a href="./#game-specific-info">Game Specific Info</a>
- <a href="./#game-specific-settings">Game Specific Settings</a>
- <a href="./#action-spaces">Action Space(s)</a>
    - <a href="./#action-space-settings">Action Space Setting</a>
- <a href="./#observation-space">Observation Space</a>
    - <a href="./#global">Global</a>
    - <a href="./#player-specific">Player Specific</a>
- <a href="./#reward-function">Reward Function</a>


</div>

This page describes in details all general aspects related to DIAMBRA Arena environments. For game-specific details visit <a href="./games">Games & Specifics</a> page.


### Overview

DIAMBRA Arena is a software package featuring a collection of high-quality environments for Reinforcement Learning research and experimentation. It acts as an interface towards popular arcade emulated video games, offering a Python API fully compliant with OpenAI Gym standard, that makes its adoption smooth and straightforward.
                                                                                
It supports all major Operating Systems: Linux, Windows and MacOS, most of them via Docker, with a step by step installation guide available in this manual. It is completely free to use, the user only needs to register on the official website.
                                                                                
                                                                                
In addition, its <a href="https://github.com/diambra/diambraArena" target="_blank">GitHub repository</a> provides a collection of examples covering main use cases of interest that can be run in just a few steps.
                                                                                
#### Environments Main Features                                                 
                                                                                
All environments are episodic Reinforcement Learning tasks, with discrete actions (gamepad buttons) and observations composed by screen pixels plus additional numerical data (RAM values like characters health bars or characters stage side).
                                                                                
They all support both single player (1P) as well as two players (2P) mode, making them the perfect resource to explore all the following Reinforcement Learning sub-fields:
                                                                                
<div>                                                                           
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:auto; float:left; width:15.0%">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../images/home/AIvsCOM.png"/>
   <figcaption align="middle">Standard RL</figcaption>                          
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../images/home/AIvsAI.png"/>
   <figcaption align="middle">Competitive Multi-Agent</figcaption>              
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../images/home/AIvsHUM.png"/>
   <figcaption align="middle">Competitive Human-Agent</figcaption>              
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../images/home/SP.png"/>
   <figcaption align="middle">Self-Play</figcaption>                            
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../images/home/IL.png"/>
   <figcaption align="middle">Imitation Learning</figcaption>                   
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:auto; margin-left:1%; float:left; width:15.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../images/home/HITL.png"/>
   <figcaption align="middle">Human-in-the-Loop</figcaption>                    
  </figure>                                                                     
</div>         

#### Available Games 

Interfaced games have been selected among the most popular fighting retro-games. While sharing the same fundamental mechanics, they provide slightly different challenges, with specific features such as different type and number of characters, how to perform combos, health bars recharging, etc.
                                                                                
Whenever possible, games are released with all hidden/bonus characters unlocked.
                                                                                
Additional details can be found in their <a href="./games/">dedicated section</a>.
                                                                                
<div>                                                                           
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:auto; float:left; width:15.0%">
   <a href="./games/"><img style="margin-bottom: 20px; border-radius: 10px;" src="../images/envs/doapp.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="./games/"><img style="margin-bottom: 20px; border-radius: 10px;" src="../images/envs/sfiii3n.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="./games/"><img style="margin-bottom: 20px; border-radius: 10px;" src="../images/envs/tektagt.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="./games/"><img style="margin-bottom: 20px; border-radius: 10px;" src="../images/envs/umk3.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="./games/"><img style="margin-bottom: 20px; border-radius: 10px;" src="../images/envs/samsh5sp.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:auto; margin-left:1%; float:left; width:15.0%;">
   <a href="./games/"><img style="margin-bottom: 20px; border-radius: 10px;" src="../images/envs/kof98umh.jpg"/></a>
  </figure>                                                                     
</div>    

### Interaction Basics

DIAMBRA Arena Environments usage follows the standard RL interaction framework: the agent sends an action to the environment, which process it and performs a transition accordingly, from the starting state to the new state, returning the observation and the reward to the agent to close the interaction loop. The figure below shows this typical interaction scheme and data flow.

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto; width: 80%;">
  <img src="../images/envs/basicUsage.png" style="margin-bottom:20px;">
  <figcaption align="middle">Scheme of Agent-Environment Interaction</figcaption>
</figure>

The shortest snippet for a complete basic execution of an environment consists of just a few lines of code, and is presented in the code block below:

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

{{% notice note %}}
More complex and complete examples can be found in the <a href="../gettingstarted/examples/">Examples</a> section.
{{% /notice %}}

### Settings

All environments share a numerous set of options allowing to handle many different aspects, controlled by key-value pairs in a Python dictionary passed to the environment creation method, as shown on line 8 in previous code example also reported below:

```python
env = diambraArena.make("MyEnv", settings)
```

Next table summarizes and describes the general, game-independent, settings, while the game-specific ones are presented in the game dedicated pages. Settings that are shared among all games, are found in the table contained in the <a href="./#game-specific-settings">Game Specific Settings</a> section below.

{{% notice tip %}}
Two ready-to-use examples showing how environment settings are used can be found in the Examples folder inside DIAMBRA Arena repository, and described in <a href="../gettingstarted/examples/singleplayerenv/">this</a> and <a href="../gettingstarted/examples/multiplayerenv/">this section</a> of this manual.
{{% /notice %}}

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

Game specific details provide useful information about each title. They are reported in every game-dedicated page, and summarized and described in the table below.

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

Environment settings depending on the specific game and shared among all of them are reported in the table below. Additional ones (if present) are reported in game-dedicated pages.

| <strong><span style="color:#5B5B60;">Setting</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> |
|-------------|-------------| ------|------|-----|
| <strong><span style="color:#5B5B60;">Difficulty (1P Mode Only)</span></strong>   | `difficulty`       | `int`| Default value if not specified | Min and Max values allowed for the specific game|
| <strong><span style="color:#5B5B60;">Characters List</span></strong>   | `characters`| `string`       | Default characters if not specified (for both P1 and P2) | List of characters games that can be selected for the specific game |
| <strong><span style="color:#5B5B60;">Characters Outfits</span></strong>   | `charOutfits`| `int`      | Default values if not specified (for both P1 and P2) | Min and Max values allowed for the specific game |

<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;width: 60%">
  <img src="../images/envs/outfits.png" style="margin-bottom:20px;">
  <figcaption align="middle">Example of Dead or Alive ++ available outfits for Kasumi</figcaption>
</figure>

### Action Space(s)

Actions of the interfaced games can be grouped in two categories: move actions (Up, Left, etc.) and attack ones (Punch, Kick, etc.). DIAMBRA Arena provides four different action spaces: the main distinction is between Discrete and MultiDiscrete ones. The former is a single list composed by the union of move and attack actions (of type `gym.spaces.Discrete`), while the latter consists of two sets combined, for move and attack actions respectively (of type `gym.spaces.MultiDiscrete`). 

For each of the two options, there is an additional differentiation available: if to use attack buttons combinations or not. This option is mainly available to reduce the action space size as much as possible, since combinations of attack buttons can be seen as additional attack buttons. The complete visual description of available action spaces is shown in the figure below, where all four choices are presented via the correspondent gamepad buttons configuration for Dead Or Alive ++.

When run in 2P mode, the environment is provided with an action space of `type gym.spaces.Dict` populated with two items, identified by keys "P1" and "P2", whose values are either `gym.spaces.Discrete` or `gym.spaces.MultiDiscrete` as described above. 

Each game has specific action spaces since attack buttons (and their combinations) are, in general, game-dependent. For this reason, in each game-dedicated page, a table like the one found below is reported, describing all four actions spaces for the specific game.

In Discrete action spaces: 
- There is only one ”no-op” action, that covers both the ”no-move” and ”no-attack” actions.
- The total number of actions available is N<sub>m</sub> + N<sub>a</sub> − 1 where N<sub>m</sub> is the number of move actions (no-move included) and N<sub>a</sub> is the number of attack actions (no-attack included).
Only one action, either move or attack, can be sent for
each environment step.

In MultiDiscrete action spaces:
- There is only one ”no-op” action, that covers both the ”no-move” and ”no-attack” actions. 
- The total number of actions available is N<sub>m</sub> × N<sub>a</sub>.
- Both move and attack actions can be sent at the same time for each environment step. 

All meaningful actions are made available per each game: they are sufficient to cover the entire spectrum of moves and combos for all the available characters.

Only meaningful actions are made available per each game: if a specific game has Button-1 and Button-2 among its available actions, and not Button-1 + Button-2, it means that the latter has no effect in any circumstance, considering all characters in all conditions.

Some actions (especially attack buttons combinations) may have no effect for some of the characters: in some games combos requiring attack buttons combinations are valid only for a subset of characters.

<figure style="margin-bottom:20px; margin-top:0px; margin-right:auto; margin-left:auto;width: 60%;">
  <img src="../images/envs/actionSpaces.png" style="margin-bottom:20px;">
  <figcaption align="middle">Example of Dead Or Alive ++ Action Spaces</figcaption>
</figure>

#### Action Space Settings

| <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Attack Buttons<br>Combination</span></strong> | <strong><span style="color:#5B5B60;">Keys</span></strong> | <strong><span style="color:#5B5B60;">Values</span></strong>| <strong><span style="color:#5B5B60;">Space Size (Number of Actions)</span></strong> |
|-------------|-------------| ------|-------| ------- |
| <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> / <a href="https://github.com/openai/gym/tree/master/gym/spaces/multi_discrete.py" target="blank_">MultiDiscrete</a>  | Active / Not Active  |`actionSpace`,  `attackButCombination` | `discrete`/`multiDiscrete`, `True`/`False` | Total number of actions available, divided in move and attack actions |

### Observation Space

Environment observations are composed by two main elements: a visual one (the game frame) and an aggregation of quantitative values called Additional Observations (stage number, health values, etc.). Both of them are exposed through an observation space of type <a href="https://github.com/openai/gym/tree/master/gym/spaces/" target="blank_">`gym.spaces.Dict`</a>. It consists of global elements and player-specific ones, they are presented and described in the tables below. To give additional context, next figure shows an example of Dead Or Alive ++ observation where some of the Additional Observations are highlighted, superimposed on the game frame.

Each game specifies and extends the set presented here with its custom one, described in the game-dedicated page.


<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <img src="../images/envs/doappData.png" style="margin-bottom:20px;">
  <figcaption align="middle">An example of Dead Or Alive ++ additional observations</figcaption>
</figure>

#### Global

Global elements of the observation space are unrelated to the player and they are currently limited to those presented and described in the following table. The same table is found on each game-dedicated page reporting its specs:

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Frame</span></strong>   | `frame`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a> | Min and max values for each dimension | Last game frame  (RGB pixel screen)|
| <strong><span style="color:#5B5B60;">Stage (1P Mode Only)</span></strong>   | `stage` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  Min and max values | Current stage of the game |

#### Player specific

Player-specific observations can be accessed using key(s) `P1` (1P and 2P Modes) and/or `P2` (2P Mode only), as shown in the following snippet for the <strong><span style="color:#5B5B60;">Side</span></strong> element:

```python
ownSideVar = observation["P1"]["ownSide"]
```

Typical values that are available for each game are reported and described in the table below. The same table is found in every game-dedicated page, specifying and extending (if needed) the observation elements set.

| <strong><span style="color:#5B5B60;">Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|-------| --------------|
| <strong><span style="color:#5B5B60;">Side</span></strong>   | `ownSide`/`oppSide`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> (Binary) | [0,&#160;1] | Side of the stage where the player is<br>0: Left, 1: Right |
| <strong><span style="color:#5B5B60;">Wins</span></strong>   | `ownWins`/`oppWins` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;Max Number of Rounds]| Number of rounds won by the player |
| <strong><span style="color:#5B5B60;">Selected Character&#160;#1</span></strong>   | `ownChar1`/`oppChar1`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;Max Number of Characters - 1] | Index of character in use (for games where only one character is selected, this values is the same as "Character in Use")|
| <strong><span style="color:#5B5B60;">Character in Use</span></strong>   | `ownChar`/`oppChar`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;Max Number of Characters - 1] | Index of character in use|
| <strong><span style="color:#5B5B60;">Health</span></strong>   | `ownHealth`/`oppHealth` | <a href="https://github.com/openai/gym/tree/master/gym/spaces/box.py" target="blank_">Box</a>   |  [0,&#160;Max Health Value]| Health bar value |
| <strong><span style="color:#5B5B60;">Actions-Move</span></strong>   | `actions`+`move`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;Max number of move actions - 1] | Index of last move action performed (no-move, left, left+up, up, etc.)|
| <strong><span style="color:#5B5B60;">Actions-Attack</span></strong>   | `actions`+`attack`       | <a href="https://github.com/openai/gym/tree/master/gym/spaces/discrete.py" target="blank_">Discrete</a> | [0,&#160;Max number of attack actions - 1] | Index of last attack action performed (no-attack, hold, punch, etc.) with, respectively, attack buttons combination active or not|

### Reward Function

The reward is defined as a function of characters health values so that, qualitatively, damage suffered by the agent corresponds to a negative reward, and damage inflicted to the opponent corresponds to a positive reward. The quantitative, general and formal reward function definition is as follows: 

$$
\begin{equation}
R_t = \frac{\sum_i^{0,N_c}\left(\bar{H_i}^{t^-} - \bar{H_i}^{t} - \left(\hat{H_i}^{t^-} - \hat{H_i}^{t}\right)\right)}{\left(H_{max}-H_{min}\right)/2}
\end{equation}
$$

Where:

- $\bar{H}$ and $\hat{H}$ are health values for opponent’s character(s) and agent’s one(s) respectively; 
- $t^-$ and $t$ are used to indicate conditions at ”state-time” and at ”new state-time” (i.e. before and after environment step); 
- $N_c$ is the number of characters taking part in a round. Usually is $N_c = 1$ but there are some games where multiple characters are used, with the additional possible option of alternating them during gameplay, like Tekken Tag Tournament where 2 characters have to be selected and two opponents are faced every round (thus $N_c = 2$); 
- $H_{max}$ and $H_{min}$ are maximum and minimum health values, respectively, for the given game; usually, but not always, $H_{min} = 0$.

The normalization term at the denominator in reward equation (Eq. 1) ensures that a round won with a perfect (i.e. without losing any health), generates a maximum total cumulative reward (for the round) equal to $2N_c$.

The lower and upper bounds for the episode total cumulative reward are defined in the equations (Eqs. 2) below. They consider the default reward function for game execution with Continue Game option set equal to 0.0 (Continue not allowed).

$$
\begin{equation}
\begin{gathered}
\min{\sum_t^{0,T_s}R_t} = -2 N_c \left( \left(N_s-1\right) \left(N_r-1\right) +  N_r\right) \\\\
\max{\sum_t^{0,T_s}R_t} = 2 N_c N_s N_r
\end{gathered}
\end{equation}
$$

Where:
- $N_r$ is the number of rounds to win (or lose) in order to win (or lose) a stage;
- $T_s$ is the terminal state, reached when either $N_r$ rounds are lost (for both 1P and 2P mode) or game is cleared (for 1P mode only); 
- $t$ represents the environment step and for an episode goes from 0 to $T_s$;
- $N_s$ is the maximum number of stages the agent can play before the game reaches $T_s$. 

For 1P mode $N_s$ is game-dependent, while for 2P mode $N_s=1$, meaning the episode always ends after a single stage (so after $N_r$ rounds have been won / lost be the same player, either P1 or P2).

For 2P mode, P1 reward is defined as $R$ in the reward Eq. 1 and P2 reward is equal to $-R$ (zero-sum games). Eq. 1 describes the default reward function. It is of course possible to tweak it at will by means of custom <a href="../wrappers/#reward-wrappers">Reward Wrappers</a>.

The minimum and maximum total cumulative reward for the round can be <strong><span style="color:#5B5B60;">different than</span></strong> $2N_c$ in some cases. This may happen because:
 - When multiple characters are used at the same time, the "Round Done" condition can be different for different games (e.g. either at least one character has zero health or all characters have zero health) impacting on the amount of reward collected.
 - For some games health bars can be recharged (e.g. the character in background in Tekken Tag Tournament, or Gill's resurrection move in Street Fighter III), making available an extra amount of reward to be collected or lost in that round.
 - For some games, in some stages, additional opponents may be faced (opponent $N_c$ not constant through stages), making available an extra amount of reward to be collected (e.g. the endurance stages in Ultimate Mortal Kombat 3).
 - For some games, not all characters share the same maximum health. $H_{max}$ and $H_{min}$ are always the extremes for a given game, among all characters.

Lower and upper bounds of episode total cumulative reward may, in some cases, deviate from what defined by Eqs. 2, because:
- The absolute value of minimum / maximum total cumulative reward for the round can be different from $2N_c$ (see above).
- For some games, $N_r$ is not the same for all the stages (1P mode only), for example for Tekken Tag Tournament the final stage is made of a single round while all previous ones require two wins.

Please note that the maximum cumulative reward (for 1P mode) is obtained when clearing the game winning all rounds with a perfect ($\max{\sum_t^{0,T_s}R_t}\Rightarrow$ game completed), but the vice versa is not true. In fact not necessarily the higher number of stages won, the higher is the total cumulative reward ($\max{\sum_t^{0,T_s}R_t}\not\propto$ stage reached, game completed $\nRightarrow\max{\sum_t^{0,T_s}R_t}$). Somehow counter intuitively, in order to obtain the lowest possible total cumulative reward the agent is supposed to reach the final stage (collecting negative rewards in all previous ones) before loosing by $N_r$ perfects.
