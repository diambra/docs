---
date: 2016-04-09T16:50:16+02:00
title: Human Experience Recorder
weight: 50
---

This example focuses on:

- Human experience recording settings configuration
- Controller interfacing (Gamepad or Keyboard)

{{% notice tip %}}
A dedicated section describing human experience recording wrapper settings is presented <a href="/imitationlearning/#experience-recording-wrapper">here</a> and provides additional details on their usage and purpose.
{{% /notice %}}

{{% notice note %}}
Depending on the Operating System used, specific permissions may be needed in order to read the keyboard inputs.<br><br> - On Windows, by default no specific permissions are needed. However, if you have some third-party security software you may need to white-list Python.<br> - On Linux you need to add the user the `input` group: `sudo usermod -aG input $USER`<br> - On Mac, it is possible you need to use the settings application to allow your program to access the input devices (see <a href="https://inputs.readthedocs.io/en/latest/user/install.html#mac-permissions" target="_blank">this reference</a>).<br><br>Official `inputs` python package reference guide can be found at <a href="https://inputs.readthedocs.io/en/latest/user/install.html#windows-permissions" target="_blank">this link</a>
{{% /notice %}}

#### Modules import

```python
import os
from os.path import expanduser
import diambra.arena
from diambra.arena.utils.controller import get_diambra_controller
```

#### Settings

```python
# Environment Settings
settings = {}
settings["player"] = "Random"
settings["step_ratio"] = 1
settings["frame_shape"] = (128, 128, 1)
settings["action_space"] = "multi_discrete"
settings["attack_but_combination"] = True

# Gym wrappers settings
wrappers_settings = {}
wrappers_settings["reward_normalization"] = True
wrappers_settings["frame_stack"] = 4
wrappers_settings["actions_stack"] = 12
wrappers_settings["scale"] = True
```

#### Experience recording settings

```python
# Gym trajectory recording wrapper kwargs
traj_rec_settings = {}
home_dir = expanduser("~")

# Username
traj_rec_settings["username"] = "Alex"

# Path where to save recorderd trajectories
game_id = "doapp"
traj_rec_settings["file_path"] = os.path.join(home_dir, "diambraArena/trajRecordings", game_id)

# If to ignore P2 trajectory (useful when collecting
# only human trajectories while playing as a human against a RL agent)
traj_rec_settings["ignore_p2"] = False
```

#### Envionment execution

```python
env = diambra.arena.make(game_id, settings, wrappers_settings, traj_rec_settings)

# Controller initialization
controller = get_diambra_controller(env.action_list)
controller.start()

observation = env.reset()

while True:

    env.render()

    actions = controller.get_actions()

    observation, reward, done, info = env.step(actions)

    if done:
        observation = env.reset()
        break

controller.stop()
env.close()
```
