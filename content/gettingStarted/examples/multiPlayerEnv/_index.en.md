---
date: 2016-04-09T16:50:16+02:00
title: Multi Player Environment
weight: 20
---

This example focuses on:
 - Two players mode environment usage (Competitive Multi-Agent, SelfPlay, Competitive Human-Agent)
 - Environment settings configuration
 - Algorithms - Environment interaction loop
 - Gym observation visualization

{{% notice tip %}}
A dedicated section describing environment settings is presented <a href="/envs/#settings">here</a> and provides additional details on their usage and purpose.
{{% /notice %}}

#### Modules import

```python
import diambraArena
from diambraArena.gymUtils import showGymObs
import numpy as np
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
# 2 Players game
settings["player"] = "P1P2"
```

#### Action space settings

```python
# If to use discrete or multiDiscrete action space
settings["actionSpace"] = ["discrete", "discrete"]

# If to use attack buttons combinations actions
settings["attackButCombination"] = [True, True]
```

#### Environment execution

```python
env = diambraArena.make("TestEnv", settings)

observation = env.reset()
showGymObs(observation, env.charNames)

while True:

    actions = env.action_space.sample()
    actions = np.append(actions["P1"], actions["P2"])

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
