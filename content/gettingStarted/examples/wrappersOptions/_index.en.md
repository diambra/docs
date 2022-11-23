---
date: 2016-04-09T16:50:16+02:00
title: Wrappers Options
weight: 40
---

This example focuses on:

- Wrappers settings configuration
- Wrapped observation visualization

{{% notice tip %}}
A dedicated section describing environment wrappers settings is presented <a href="/wrappers/">here</a>, while more info on wrapped observation visualization utils are presented <a href="/utils/#wrapped-observation">here</a>. They both provide additional details on usage and purpose.
{{% /notice %}}

#### Modules import

```python
import diambra.arena
```

#### Wrappers settings

```python
# Gym wrappers settings
wrappers_settings = {}

# Number of no-Op actions to be executed
# at the beginning of the episode (0 by default)
wrappers_settings["no_op_max"] = 0

# Number of steps for which the same action should be sent (1 by default)
wrappers_settings["sticky_actions"] = 1

# Frame resize operation spec (deactivated by default)
# WARNING: for speedup, avoid frame warping wrappers,
#          use environment's native frame wrapping through
#          "frame_shape" setting (see documentation for details).
wrappers_settings["hwc_obs_resize"] = (128, 128, 1)

# Wrapper option for reward normalization
# When activated, the reward normalization factor can be set (default = 0.5)
# The normalization is performed by dividing the reward value
# by the product of the factor times the value of the full health bar
# reward = reward / (C * fullHealthBarValue)
wrappers_settings["reward_normalization"] = True
wrappers_settings["reward_normalization_factor"] = 0.5

# If to clip rewards (False by default)
wrappers_settings["clip_rewards"] = False

# Number of frames to be stacked together (1 by default)
wrappers_settings["frame_stack"] = 4

# Frames interval when stacking (1 by default)
wrappers_settings["dilation"] = 1

# How many past actions to stack together (1 by default)
wrappers_settings["actions_stack"] = 12

# If to scale observation numerical values (deactivated by default)
# optionally exclude images from normalization (deactivated by default)
# and optionally perform one-hot encoding also on discrete binary variables (deactivated by default)
wrappers_settings["scale"] = True
wrappers_settings["exclude_image_scaling"] = True
wrappers_settings["process_discrete_binary"] = True

# Scaling interval (0 = [0.0, 1.0], 1 = [-1.0, 1.0])
wrappers_settings["scale_mod"] = 0

# Flattening observation dictionary and filtering
# a sub-set of the RAM states
wrappers_settings["flatten"] = True
wrappers_settings["filter_keys"] = ["stage", "P1_ownSide", "P1_oppSide",
                                    "P1_ownHealth", "P1_oppChar",
                                    "P1_actions_move", "P1_actions_attack"]
```

#### Environment execution

```python
env = diambra.arena.make("doapp", {}, wrappers_settings)

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
