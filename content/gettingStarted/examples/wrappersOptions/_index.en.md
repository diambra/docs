---
date: 2016-04-09T16:50:16+02:00
title: Wrappers Options
weight: 40
---

This example focuses on:
 - Wrapppers settings configuration
 - Wrapped observation visualization

{{% notice tip %}}
A dedicated section describing environment wrappers settings is presented <a href="/wrappers/">here</a>, while more info on wrapped observation visualization utils are presented <a href="/utils/#wrapped-observation">here</a>. They both provide additional details on usage and purpose. 
{{% /notice %}}

#### Modules import

```python
import diambraArena
from diambraArena.gymUtils import showWrapObs
```

#### Mandatory settings

```python
settings = {}

# Game selection
settings["gameId"] = "doapp"

# Path to roms folder
settings["romsPath"] = "home/user/DIAMBRA/roms/"
```

#### Wrappers settings

```python
# Gym wrappers settings
wrappersSettings = {}

# Number of no-Op actions to be executed at the beginning of the episode (0 by default)
wrappersSettings["noOpMax"] = 0

# Number of steps for which the same action should be sent (1 by default)
wrappersSettings["stickyActions"] = 1

# Frame resize operation spec (deactivated by default)
wrappersSettings["hwcObsResize"] = [128, 128, 1]

# Reward normalization factor
# activates normalization if different from 1.0 (1.0 by default)
wrappersSettings["rewardNormalizationFactor"] = 0.5

# If to clip rewards (False by default)
wrappersSettings["clipRewards"] = False

# Number of frames to be stacked together (1 by default)
wrappersSettings["frameStack"] = 4

# Frames interval when stacking (1 by default)
wrappersSettings["dilation"] = 1

# How many past actions to stack together (1 by default)
wrappersSettings["actionsStack"] = 12

# If to scale observation numerical values (deactivated by default)
wrappersSettings["scale"] = True

# Scaling interval (0 = [0.0, 1.0], 1 = [-1.0, 1.0])
wrappersSettings["scaleMod"] = 0
```

#### Environment execution


```python
env = diambraArena.make("TestEnv", settings, wrappersSettings)

observation = env.reset()
showWrapObs(observation, env.nActionsStack, env.charNames)

while True:

    actions = env.action_space.sample()

    observation, reward, done, info = env.step(actions)
    showWrapObs(observation, env.nActionsStack, env.charNames)
    print("Reward: {}".format(reward))
    print("Done: {}".format(done))
    print("Info: {}".format(info))

    if done:
        observation = env.reset()
        showWrapObs(observation, env.nActionsStack, env.charNames)
        break

env.close()
```
