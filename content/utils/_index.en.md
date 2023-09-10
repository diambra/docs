---
title: Utils
weight: 50
---

### Index

<div style="font-size:1.125rem;">

- <a href="./#available-games">Available Games</a>
- <a href="./#roms-check">ROMs Check</a>
- <a href="./#environment-spaces-summary">Environment Spaces Summary</a>
- <a href="./#observation-inspection">Observation Inspection</a>
    - <a href="./#base-environment">Base Environment</a>
    - <a href="./#wrapped-observation">Wrapped Observation</a>
- <a href="./#controller-interface">Controller Interface</a>

</div>

DIAMBRA Arena comes with many different tools supporting development and debug. They provide different functionalities, all described below in the sections below where both code and output is reported.

Source code can be found in the code repository, <a href="https://github.com/diambra/arena/tree/main/diambra/arena/utils/gym_utils.py" target="_blank">here</a>.

### Available Games

Provides a list of available games and their most important details. It is executed as shown below:

```python
import diambra.arena
diambra.arena.available_games(print_out=True, details=True)
```

Output will be similar to what follows:

```txt
 Title: Dead Or Alive ++ - GameId: doapp
   Difficulty levels: Min 1 - Max 4
   SHA256 sum: d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
   Original ROM name: doapp.zip
   Search keywords: ['DEAD OR ALIVE ++ [JAPAN]', 'dead-or-alive-japan', '80781', 'wowroms']
   Characters list: ['Kasumi', 'Zack', 'Hayabusa', 'Bayman', 'Lei-Fang', 'Raidou', 'Gen-Fu', 'Tina', 'Bass', 'Jann-Lee', 'Ayane']

 Title: Street Fighter III - GameId: sfiii3n
   Difficulty levels: Min 1 - Max 8
   SHA256 sum: 7239b5eb005488db22ace477501c574e9420c0ab70aeeb0795dfeb474284d416
   Original ROM name: sfiii3n.zip
   Search keywords: ['STREET FIGHTER III 3RD STRIKE: FIGHT FOR THE FUTUR [JAPAN] (CLONE)', 'street-fighter-iii-3rd-strike-fight-for-the-futur-japan-clone', '106255', 'wowroms']
   Characters list: ['Alex', 'Twelve', 'Hugo', 'Sean', 'Makoto', 'Elena', 'Ibuki', 'Chun-Li', 'Dudley', 'Necro', 'Q', 'Oro', 'Urien', 'Remy', 'Ryu', 'Gouki', 'Yun', 'Yang', 'Ken', 'Gill']

 Title: Tekken Tag Tournament - GameId: tektagt
   Difficulty levels: Min 1 - Max 9
   SHA256 sum: 57be777eae0ee9e1c035a64da4c0e7cb7112259ccebe64e7e97029ac7f01b168
   Original ROM name: tektagtac.zip
   Search keywords: ['TEKKEN TAG TOURNAMENT [ASIA] (CLONE)', 'tekken-tag-tournament-asia-clone', '108661', 'wowroms']
   Notes: Rename the rom from tektagtac.zip to tektagt.zip
   Characters list: ['Xiaoyu', 'Yoshimitsu', 'Nina', 'Law', 'Hwoarang', 'Eddy', 'Paul', 'King', 'Lei', 'Jin', 'Baek', 'Michelle', 'Armorking', 'Gunjack', 'Anna', 'Brian', 'Heihachi', 'Ganryu', 'Julia', 'Jun', 'Kunimitsu', 'Kazuya', 'Bruce', 'Kuma', 'Jack-Z', 'Lee', 'Wang', 'P.Jack', 'Devil', 'True Ogre', 'Ogre', 'Roger', 'Tetsujin', 'Panda', 'Tiger', 'Angel', 'Alex', 'Mokujin', 'Unknown']
...
```

### ROMs Check

Checks ROM SHA256 checksum to validate them.

#### Without `game_id` specification

If no `game_id` is specified, the ROM file provided as first argument, will be verified against all available games. It is executed as shown below:

```python
import diambra.arena
diambra.arena.check_game_sha_256(path="path/to/specific/rom/doapp.zip", game_id=None)
```

Output will be similar to what follows:

```txt
Correct ROM file for Dead Or Alive ++, sha256 = d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
```

#### With `game_id` specification

If `game_id` is specified, the checksum of the ROM file provided will be compared with the one of the specified game identified by `game_id`. It is executed as shown below:

```python
import diambra.arena
diambra.arena.check_game_sha_256(path="path/to/specific/rom/doapp.zip", game_id="umk3")
```
Output will be similar to what follows:

```txt
Expected  SHA256 Checksum: f48216ad82f78cb86e9c07d2507be347f904f4b5ae354a85ae7c34d969d265af
Retrieved SHA256 Checksum: d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
```

### Environment Spaces Summary

Prints out a summary of Environment's Observation and Action spaces showing nesting levels, keys, space types and low/high bounds where applicable. It is executed as shown below:

```python
from diambra.arena.gym_utils import env_spaces_summary
...
env_spaces_summary(env=environment)
```
Output will be similar to what follows:

```txt
Observation space:

observation_space["action_attack"]: Discrete(4)

observation_space["action_move"]: Discrete(9)

observation_space["opp_char"]: Discrete(11)

observation_space["opp_health"]: Box()
    Space type = int32
    Space high bound = 208
    Space low bound = 0

observation_space["opp_side"]: Discrete(2)

observation_space["opp_wins"]: Box()
    Space type = int32
    Space high bound = 2
    Space low bound = 0

observation_space["own_char"]: Discrete(11)

observation_space["own_health"]: Box()
    Space type = int32
    Space high bound = 208
    Space low bound = 0

observation_space["own_side"]: Discrete(2)

observation_space["own_wins"]: Box()
    Space type = int32
    Space high bound = 2
    Space low bound = 0

observation_space["frame"]: Box(480, 512, 3)
        Space type = uint8
        Space high bound = [[[255 255 255]
  [255 255 255]
  [255 255 255]
  ...
  [255 255 255]
  [255 255 255]
  [255 255 255]]

 ...

 [[255 255 255]
  [255 255 255]
  [255 255 255]
  ...
  [255 255 255]
  [255 255 255]
  [255 255 255]]]
        Space low bound = [[[0 0 0]
  [0 0 0]
  [0 0 0]
  ...
  [0 0 0]
  [0 0 0]
  [0 0 0]]

 ...

 [[0 0 0]
  [0 0 0]
  [0 0 0]
  ...
  [0 0 0]
  [0 0 0]
  [0 0 0]]]

observation_space["stage"]: Box()
        Space type = int8
        Space high bound = 8
        Space low bound = 1

observation_space["timer"]: Box()
        Space type = int8
        Space high bound = 40
        Space low bound = 0

Action space:

action_space =  Discrete(12)
  Space type =  int64
    Space n =  12
```

### Observation Inspection

Prints out a detailed description of Environment's observation content, showing every level of it. The only element that is not printed in the terminal is the game frame that, when `viz` input argument is set to `True`, is shown in dedicated graphical windows, one per channel (i.e. RGB channels are shown separately). The `wait_key` parameter defines how this window(s) behaves: when set equal to 0, it pauses waiting for the user to press a button, while if set different from zero, it waits the prescribed number of milliseconds before continuing.

The same method works on both the basic Gym Environment and the wrapped one.

```python
env.show_obs(observation=obs, wait_key=1, viz=True)
```

{{% notice tip %}}
Use of this functionality can be found in <a href="../gettingstarted/examples/singleplayerenv/">this</a>, <a href="../gettingstarted/examples/multiplayerenv/">this</a>, <a href="../gettingstarted/examples/wrappersoptions/">this</a> and <a href="../gettingstarted/examples/datasetloader/">this</a> examples.
{{% /notice %}}

#### Base Environment

Output will be similar to what follows:

 - Frame(s) visualization window(s):
<figure style="margin-right:auto; margin-left:auto;">
  <img src="../images/utils/gymObs.jpg" style="padding-left:40px;margin-bottom:1rem; margin-top:0px">
</figure>

 - Terminal printout:
   ```txt
   observation["frame"]: shape (128, 128, 3) - min 0 - max 255
   observation["stage"]: [1]
   observation["timer"]: [30]
   observation["own_char"]: 0 / Kasumi
   observation["opp_char"]: 3 / Bayman
   observation["own_health"]: [66]
   observation["opp_health"]: [184]
   observation["own_side"]: 0
   observation["opp_side"]: 1
   observation["own_wins"]: [0]
   observation["opp_wins"]: [0]
   observation["action_move"]: 0
   observation["action_attack"]: 3
   ```

#### Wrapped Observation

Output will be similar to what follows:

- Frame stack visualization windows:
<figure style="margin-right:auto; margin-left:auto;">
  <img src="../images/utils/wrapObs.jpg" style="padding-left:40px;margin-bottom:1rem; margin-top:0px">
</figure>

- Terminal printout:

  ```txt
  observation["frame"]: shape (128, 128, 4) - min 0 - max 255
  observation["stage"]: [0.0]
  observation["timer"]: [0.875]
  observation["own_char"]: [0 0 0 1 0 0 0 0 0 0 0] / Bayman
  observation["opp_char"]: [1 0 0 0 0 0 0 0 0 0 0] / Kasumi
  observation["own_health"]: [0.8173076923076923]
  observation["opp_health"]: [0.8028846153846154]
  observation["own_side"]: [0 1]
  observation["opp_side"]: [1 0]
  observation["own_wins"]: [0.]
  observation["opp_wins"]: [0.]
  observation["action_move"] (reshaped for visualization):
  [[1 0 0 0 0 0 0 0 0]
   [1 0 0 0 0 0 0 0 0]
   [0 0 0 0 0 0 0 0 1]
   [1 0 0 0 0 0 0 0 0]
   [0 1 0 0 0 0 0 0 0]
   [0 1 0 0 0 0 0 0 0]
   [1 0 0 0 0 0 0 0 0]
   [1 0 0 0 0 0 0 0 0]
   [0 0 0 0 0 0 1 0 0]
   [0 0 0 0 0 0 1 0 0]
   [0 0 0 1 0 0 0 0 0]
   [0 0 0 0 0 0 0 0 1]]
  observation["action_attack"]  (reshaped for visualization):
  [[1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [0 0 1 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]
   [1 0 0 0]]
  ```

### Controller Interface

It allows to easily interface a common USB Gamepad or the Keyboard to DIAMBRA Arena environments, to be used for experiments where human input is required, for example Human Expert Demonstration Collection or Competitive Human-Agent. The following code snippet shows a typical usage.

```python
import diambra.arena
from diambra.arena.utils.controller import get_diambra_controller
...
# Environment Initialization
...
# Controller initialization
controller = get_diambra_controller(env.get_actions_tuples())
controller.start()
...
# Player-Environment interaction loop
while condition:
    ...
    actions = controller.get_actions()
    ...
...
controller.stop()
```

{{% notice tip %}}
Use of this functionality can be found in <a href="../gettingstarted/examples/episoderecorder/">this</a> example.
{{% /notice %}}

{{% notice note %}}
Depending on the Operating System used, specific permissions may be needed in order to read the keyboard inputs.<br><br> - On Windows, by default no specific permissions are needed. However, if you have some third-party security software you may need to white-list Python.<br> - On Linux you need to add the user the `input` group: `sudo usermod -aG input $USER`<br> - On Mac, it is possible you need to use the settings application to allow your program to access the input devices (see <a href="https://inputs.readthedocs.io/en/latest/user/install.html#mac-permissions" target="_blank">this reference</a>).<br><br>Official `inputs` python package reference guide can be found at <a href="https://inputs.readthedocs.io/en/latest/user/install.html#windows-permissions" target="_blank">this link</a>
{{% /notice %}}