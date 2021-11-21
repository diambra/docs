---
date: 2016-04-09T16:50:16+02:00
title: Imitation Learning
weight: 60
---

#### Modules import

```python
import diambraArena
from diambraArena.gymUtils import showWrapObs
import os
import numpy as np
```

#### Imitation learning settings

```python
# Show files in folder
basePath = os.path.dirname(os.path.abspath(__file__))
recordedTrajectoriesFolder = os.path.join(basePath, "recordedTrajectories")
recordedTrajectoriesFiles = [os.path.join(recordedTrajectoriesFolder, f)
                             for f in os.listdir(recordedTrajectoriesFolder)
                             if os.path.isfile(os.path.join(recordedTrajectoriesFolder, f))]
print(recordedTrajectoriesFiles)

# Imitation learning settings
settings = {}

# List of recorded trajectories files
settings["trajFilesList"] = recordedTrajectoriesFiles

# Number of parallel Imitation Learning environments to run
settings["totalCpus"] = 2
```

#### Environment execution

```python
env = diambraArena.imitationLearning(**settings)

observation = env.reset()
env.render(mode="human")
showWrapObs(observation, env.nActionsStack, env.charNames)

# Show trajectory summary
env.trajSummary()

while True:

    dummyActions = 0
    observation, reward, done, info = env.step(dummyActions)
    env.render(mode="human")
    showWrapObs(observation, env.nActionsStack, env.charNames)
    print("Reward: {}".format(reward))
    print("Done: {}".format(done))
    print("Info: {}".format(info))

    if np.any(env.exhausted):
        break

    if done:
        observation = env.reset()
        env.render(mode="human")
        showWrapObs(observation, env.nActionsStack, env.charNames)

env.close()
```
