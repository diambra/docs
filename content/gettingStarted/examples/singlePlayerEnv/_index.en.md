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
A dedicated section describing environment settings is presented <a href="/envs/#settings">here</a>, while more info on gym observation visualization utils are presented <a href="/utils/#gym-observation">here</a>. They both provide additional details on usage and purpose.
{{% /notice %}}

#### Modules import

```python
import diambra.arena
```

#### Environment settings

```python
# Settings
settings = {}

# Player side selection: P1 (left), P2 (right), Random (50% P1, 50% P2)
settings["player"] = "P2"

# Number of steps performed by the game
# for every environment step, bounds: [1, 6]
settings["step_ratio"] = 6

# Native frame resize operation
settings["frame_shape"] = (128, 128, 0)  # RBG with 128x128 size
# settings["frame_shape"] = (0, 0, 1) # Grayscale with original size
# settings["frame_shape"] = (0, 0, 0) # Deactivated (Original size RBG)

# Game continue logic (0.0 by default):
# - [0.0, 1.0]: probability of continuing game at game over
# - int((-inf, -1.0]): number of continues at game over
#                      before episode to be considered done
settings["continue_game"] = 0.0

# If to show game final when game is completed
settings["show_final"] = False

# If to use hardcore mode in which observations are only made of game frame
settings["hardcore"] = False
```

#### Game-specific settings

```python
# Game difficulty level
settings["difficulty"] = 4

# Character to be used, automatically extended with "Random" for games
# required to select more than one character (e.g. Tekken Tag Tournament)
settings["characters"] = ("Random")

# Character outfit
settings["char_outfits"] = 2
```

#### Action space settings

```python
# Action space
# If to use discrete or multi_discrete action space
settings["action_space"] = "multi_discrete"

# If to use attack buttons combinations actions
settings["attack_but_combination"] = True
```

#### Environment execution

```python
env = diambra.arena.make("doapp", settings)

observation = env.reset()
env.show_obs(observation)

while True:

    actions = env.action_space.sample()
    print("Actions: {}".format(actions))

    observation, reward, done, info = env.step(actions)
    env.show_obs(observation)
    print("Reward: {}".format(reward))
    print("Done: {}".format(done))
    print("Info: {}".format(info))

    if done:
        observation = env.reset()
        env.show_obs(observation)
        break

env.close()
```
