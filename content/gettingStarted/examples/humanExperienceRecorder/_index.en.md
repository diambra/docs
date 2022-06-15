---
date: 2016-04-09T16:50:16+02:00
title: Human Experience Recorder
weight: 50
---

This example focuses on:
 - Human experience recording settings configuration
 - Gamepad interfacing

{{% notice tip %}}
A dedicated section describing human experience recording wrapper settings is presented <a href="/imitationlearning/#experience-recording-wrapper">here</a> and provides additional details on their usage and purpose.
{{% /notice %}}

#### Modules import

```python
import diambraArena
from diambraArena.gymUtils import showWrapObs
from diambraArena.utils.diambraGamepad import diambraGamepad
import os
from os.path import expanduser
```
#### Settings

```python
# Environment settings

settings = {}
settings["player"] = "Random"
settings["stepRatio"] = 1
settings["frameShape"] = [128, 128, 1]
settings["actionSpace"] = "multiDiscrete"
settings["attackButCombination"] = True

# Gym wrappers settings
wrappersSettings = {}
wrappersSettings["rewardNormalization"] = True
wrappersSettings["frameStack"] = 4
wrappersSettings["actionsStack"] = 12
wrappersSettings["scale"] = True
```

#### Experience recording settings

```python
# Gym trajectory recording wrapper kwargs
trajRecSettings = {}
homeDir = expanduser("~")

# Username
trajRecSettings["userName"] = "Alex"

# Path where to save recorderd trajectories
gameId = "doapp"
trajRecSettings["filePath"] = os.path.join(homeDir, "diambraArena/trajRecordings",
                                           gameId)

# If to ignore P2 trajectory (useful when collecting
# only human trajectories while playing as a human against a RL agent)
trajRecSettings["ignoreP2"] = 0
```

#### Envionment execution

```python
env = diambraArena.make(gameId, settings, wrappersSettings, trajRecSettings)

# GamePad(s) initialization
gamepad = diambraGamepad(env.actionList)
gamepad.start()

observation = env.reset()

while True:

    env.render()

    actions = gamepad.getActions()

    observation, reward, done, info = env.step(actions)

    if done:
        observation = env.reset()
        break

gamepad.stop()
env.close()
```
