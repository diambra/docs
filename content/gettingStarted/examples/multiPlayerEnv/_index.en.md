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
A dedicated section describing environment settings is presented <a href="/envs/#settings">here</a>, while more info on gym observation visualization utils are presented <a href="/utils/#gym-observation">here</a>. They both provide additional details on usage and purpose.
{{% /notice %}}

#### Modules import

```python
import diambra.arena
from diambra.arena.utils.gym_utils import show_gym_obs
import numpy as np
```
#### Environment settings

```python
# Environment settings
settings = {}

# 2 Players game
settings["player"] = "P1P2"
```

#### Action space settings

```python
# If to use discrete or multi_discrete action space
settings["action_space"] = ["discrete", "discrete"]

# If to use attack buttons combinations actions
settings["attack_but_combination"] = [True, True]
```

#### Environment execution

```python
env = diambra.arena.make("doapp", settings)

observation = env.reset()
show_gym_obs(observation, env.char_names)

while True:

    actions = env.action_space.sample()
    actions = np.append(actions["P1"], actions["P2"])

    observation, reward, done, info = env.step(actions)
    show_gym_obs(observation, env.char_names)
    print("Reward: {}".format(reward))
    print("Done: {}".format(done))
    print("Info: {}".format(info))

    if done:
        observation = env.reset()
        show_gym_obs(observation, env.char_names)
        break

env.close()
```
