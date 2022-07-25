---
date: 2016-04-09T16:50:16+02:00
title: Imitation Learning
weight: 60
---

This example focuses on:
 - Human expert demonstration loader class usage for Imitation Learning

{{% notice tip %}}
A dedicated section describing recorded experience loader is presented <a href="/imitationlearning/#recorded-experience-loader">here</a> and provides additional details on its usage and purpose.
{{% /notice %}}

#### Modules import

```python
import diambra.arena
from diambra.arena.utils.gym_utils import show_wrap_obs
import os
import numpy as np
```

#### Imitation learning settings

```python
# Show files in folder
base_path = os.path.dirname(os.path.abspath(__file__))
recorded_traj_folder = os.path.join(base_path, "recordedTrajectories")
recorded_traj_files = [os.path.join(recorded_traj_folder, f)
                       for f in os.listdir(recorded_traj_folder)
                       if os.path.isfile(os.path.join(recorded_traj_folder, f))]
print(recorded_traj_files)

# Imitation learning settings
settings = {}

# List of recorded trajectories files
settings["traj_files_list"] = recorded_traj_files

# Number of parallel Imitation Learning environments that will be run
settings["total_cpus"] = 2

# Rank of the created environment
settings["rank"] = 0
```

#### Environment execution

```python
env = diambra.arena.ImitationLearning(**settings)

observation = env.reset()
env.render(mode="human")
show_wrap_obs(observation, env.n_actions_stack, env.char_names)

# Show trajectory summary
env.traj_summary()

while True:

    dummy_actions = 0
    observation, reward, done, info = env.step(dummy_actions)
    env.render(mode="human")
    show_wrap_obs(observation, env.n_actions_stack, env.char_names)
    print("Reward: {}".format(reward))
    print("Done: {}".format(done))
    print("Info: {}".format(info))

    if np.any(env.exhausted):
        break

    if done:
        observation = env.reset()
        env.render(mode="human")
        show_wrap_obs(observation, env.n_actions_stack, env.char_names)

env.close()
```
