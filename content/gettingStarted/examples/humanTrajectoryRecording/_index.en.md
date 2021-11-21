---
date: 2016-04-09T16:50:16+02:00
title: Human Experience Recorder
weight: 50
---

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
# Mandatory settings
settings = {}
settings["gameId"]   = "doapp"
    settings["romsPath"] = opt.romsPath

# Additional settings
settings["player"] = "Random"
settings["stepRatio"] = 1

# Action space settings
settings["actionSpace"] = "multiDiscrete"
settings["attackButCombination"] = True

# Gym wrappers settings
wrappersSettings = {}
wrappersSettings["hwcObsResize"] = [128, 128, 1]
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
trajRecSettings["filePath"] = os.path.join(homeDir, "diambraArena/trajRecordings",
                                         settings["gameId"])

# If to ignore P2 trajectory (useful when collecting
# only human trajectories while playing as a human against a RL agent)
trajRecSettings["ignoreP2"] = 0
```

#### Envionment execution

```python
env = diambraArena.make("TestEnv", settings, wrappersSettings, trajRecSettings)

# GamePad(s) initialization
gamepad = diambraGamepad(env.actionList)
gamepad.start()

observation = env.reset()
showWrapObs(observation, env.nActionsStack, env.charNames, 1, False)

while True:

    actions = gamepad.getActions()

    observation, reward, done, info = env.step(actions)
    showWrapObs(observation, env.nActionsStack, env.charNames, 1, False)
    print("Reward: {}".format(reward))
    print("Done: {}".format(done))
    print("Info: {}".format(info))

    if done:
        observation = env.reset()
        showWrapObs(observation, env.nActionsStack, env.charNames, 1, False)
        break

env.close()
```
