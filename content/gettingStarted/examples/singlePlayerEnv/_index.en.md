---
date: 2016-04-09T16:50:16+02:00
title: Single Player Environment
weight: 10
---

This example focuses on:
 - Basic environment usage (Standard RL, Single Player Mode)
 - Environment settings configuration
 - Algorithm - Environment interaction loop
 - Gym observation visualization

{{% notice tip %}}
A dedicated section describing environment settings is presented <a href="/envs/#settings">here</a> and provides additional details on their usage and purpose.
{{% /notice %}}

#### Modules import

```python
import diambraArena
from diambraArena.gymUtils import showGymObs
```

#### Mandatory settings

```python
settings = {}

# Game selection
settings["gameId"] = "doapp"

# Path to roms folder
settings["romsPath"] = "home/user/DIAMBRA/roms/"
```

#### Additional environment settings

```python
# Player side selection: P1 (left), P2 (right), Random (50% P1, 50% P2)
settings["player"] = "Random"

# Renders the environment, deactivate for speedup
settings["render"] = True

# Locks execution to 60 FPS, deactivate for speedup
settings["lockFps"] = False

# Activate game sound
settings["sound"] = settings["lockFps"] and settings["render"]

# Number of steps performed by the game for every environment step, bounds: [1, 6]
settings["stepRatio"] = 6

# Allows to execute the environment in headless mode (for server-side executions)
settings["headless"] = False

# Game continue logic (0.0 by default):
# - [0.0, 1.0]: probability of continuing game at game over
# - int((-inf, -1.0]): number of continues at game over before episode to be considered done
settings["continueGame"] = -1.0

# If to show game final when game is completed
settings["showFinal"] = False
```
#### Game-specific settings

```python
# Game difficulty level
settings["difficulty"]  = 3

# Character to be used, automatically extended with "Random" for games requiring
# to select more than one character (e.g. Tekken Tag Tournament)
settings["characters"]  = [["Random"], ["Random"]]

# Character outfit
settings["charOutfits"] = [2, 2]
```

#### Action space settings

```python
# Action space
# If to use discrete or multiDiscrete action space
settings["actionSpace"] = "discrete"

# If to use attack buttons combinations actions
settings["attackButCombination"] = True
```

#### Environment execution

```python
# Environment ID, must be unique for every instance of the environment
envId = "TestEnv"

env = diambraArena.make(envId, settings)

observation = env.reset()
showGymObs(observation, env.charNames)

while True:

    actions = env.action_space.sample()

    observation, reward, done, info = env.step(actions)
    showGymObs(observation, env.charNames)
    print("Reward: {}".format(reward))
    print("Done: {}".format(done))
    print("Info: {}".format(info))

    if done:
        observation = env.reset()
        showGymObs(observation, env.charNames)
        break

env.close()
```
