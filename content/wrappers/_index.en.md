---
title: Wrappers
weight: 40
---

### Index

<div style="font-size:1.125rem;">

- <a href="./#generic-wrappers">Generic Wrappers</a>
  - <a href="./#no-attack-buttons-combinations">No Attack Buttons Combinations</a>
  - <a href="./#noop-steps-after-reset">NoOp Steps After Reset</a>
- <a href="./#observation-wrappers">Observation Wrappers</a>
  - <a href="./#warp-frame">Warp Frame</a>
  - <a href="./#stack-frames-with-optional-dilation">Stack Frames With Optional Dilation</a>
  - <a href="./#add-last-action-to-observation">Add Last Action to Observation</a>
  - <a href="./#stack-actions">Stack Actions</a>
  - <a href="./#scale-observation">Scale Observation</a>
  - <a href="./#role-relative-observation">Role-relative Observation</a>
  - <a href="./#flatten-and-filter-observation">Flatten and Filter Observation</a>
- <a href="./#action-wrappers">Action Wrappers</a>
  - <a href="./#repeat-action">Repeat Action</a>
- <a href="./#reward-wrappers">Reward Wrappers</a>
  - <a href="./#normalize-reward">Normalize Reward</a>
  - <a href="./#clip-reward">Clip Reward</a>
- <a href="./#custom-wrappers">Custom Wrappers</a>

</div>

DIAMBRA Arena comes with a large number of ready-to-use wrappers and examples showing how to apply them. They cover a wide spectrum of use cases, and also provide reference templates to develop custom ones. In order to activate wrappers, one needs to properly set the `WrapperSettings` class attributes and provide them as input to the environment creation method, as shown in the next code block. The class attributes are described in the next sections below.

```python
from diambra.arena EnvironmentSettings, WrapperSettings

# Settings specification
settings = EnvironmentSettings()

# Wrapper settings specification
wrapper_settings = WrapperSettings()
wrapper_settings.setting_1 = value_1
wrapper_settings.setting_2 = value_2

env = diambra.arena.make("doapp", settings, wrapper_settings)
```

{{% notice note %}}
Implementation examples and templates can be found in the code repository, <a href="https://github.com/diambra/arena/tree/main/diambra/arena/wrappers" target="_blank">here</a>.
{{% /notice %}}

{{% notice tip %}}
Use of these functionalities can be found in <a href="../gettingstarted/examples/wrappersoptions/">this</a> and <a href="../gettingstarted/examples/episoderecorder/">this</a> examples.
{{% /notice %}}

### Generic Wrappers

#### No Attack Buttons Combinations

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>  |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| `no_attack_buttons_combinations`                                              | `bool`                                                     | `False`                                                                     | `True` / `False`                                                     | Restricts the number of attack actions to single buttons, removing attack buttons combinations |

```python
wrappers_settings.no_attack_buttons_combinations = True
```

#### NoOp Steps After Reset

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>  |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| `no_op_max`                                              | `int`                                                     | 0                                                                     | [0,&#160;12]                                                     | Performs a maximum of _N_ No-Action steps after episode reset |

```python
wrappers_settings.no_op_max = 6
```

### Observation Wrappers

#### Warp Frame

{{% notice warning %}}
DEPRECATED: For speed, consider using the environment setting `frame_shape` that performs the same operation on the engine side, as described in <a href="../envs/#fixed-settings">this section</a>.
{{% /notice %}}

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                    |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `frame_shape`                                         | `tuple` of three `int` (H,&#160;W,&#160;C)                | (0,&#160;0,&#160;0)                                                 | H,&#160;W:&#160;[0,&#160;512]<br>C:&#160;0 or 1              | `frame`                                                                           | If active, resizes the frame and/or converts it from RGB to grayscale.<br>Combinations:<br>(0,&#160;0,&#160;0) - Deactivated;<br>(H,&#160;W,&#160;0) - RBG frame resized to H&#160;X&#160;W;<br>(0,&#160;0,&#160;1) - Grayscale frame;<br>(H,&#160;W,&#160;1) - Grayscale frame resized to H&#160;X&#160;W;<br>Keeps observation element of type _Box_, changes its shape |

```python
wrappers_settings.frame_shape = (128, 128, 1)
```

#### Stack Frames With Optional Dilation

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                               |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `stack_frames`                                            | `int`                                                     | 1                                                                     | [1,&#160;48]                                                     | `frame`                                                                           | Stacks latest _N_ frames together along the third dimension.<br>Keeps observation element of type _Box_, changes its shape |
| `dilation`                                               | `int`                                                     | 1                                                                     | [1,&#160;48]                                                     | `frame`                                                                           | Builds frame stacks adding one every _N_ frames.<br>Keeps observation element of type _Box_                                |

```python
wrappers_settings.stack_frame = 4
wrappers_settings.dilation = 1
```

#### Add Last Action to Observation

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                               |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `add_last_action`                                          | `bool`                                                     | `False`                                                                     | `True` / `False`                                                     | `action`                                                 | Adds latest action to the observation space dictionary, under the key `action`.<br>Adds an observation element of type _Discrete_ or _MultiDiscrete_ depending on the action space |

```python
wrappers_settings.add_last_action = True
```

#### Stack Actions

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                               |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `stack_actions`                                          | `int`                                                     | 1                                                                     | [1,&#160;48]                                                     | `action`                                                 | Stacks latest _N_ actions together.<br>Changes observation element type from _Discrete_ or _MultiDiscrete_ (depending on the action space) to _MultiDiscrete_, having _N_ elements, each with cardinality equal to the original set(s) of the action space |

```python
wrappers_settings.stack_actions = 12
```

{{% notice note %}}
For the Stack Actions wrapper to to be applied, the Add Last Action one must be activated.
{{% /notice %}}

#### Scale Observation

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `scale`                                                  | `bool`                                                    | False                                                                 | True / False                                                     | All                                                                             | Activates observation scaling.<br>Affects observation elements as follows:<br> - _Box_: type kept, changes bounds to be between 0 and 1.<br> - _Discrete (Binary)_: depends on `process_discrete_binary` (see below).<br> - _Discrete_: changed to _MultiBinary_ through one-hot encoding, size equal to original _Discrete_ cardinality.<br> - _MultiDiscrete_: changed to _N-MultiBinary_ through N-times one-hot encoding, N equal to original _MultiDiscrete_ size, encoding size equal to original _MultiDiscrete_ element cardinality |
| `exclude_image_scaling`                                  | `bool`                                                    | False                                                                 | True / False                                                     | Frame                                                                           | If True, prevents scaling to be applied on the game frame                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `process_discrete_binary`                                | `bool`                                                    | False                                                                 | True / False                                                     | Discrete Binary                                                                 | Controls how _Discrete (Binary)_ observations are treated: <br> - False: not affected; <br> - True: changed to _MultiBinary_ through one-hot encoding, size equal to 2                                                                                                                                                                                                                                                                                                                                                                                 |

```python
wrappers_settings.scale = True
wrappers_settings.exclude_image_scaling = True
wrappers_settings.process_discrete_binary = True
```

#### Role-relative Observation

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                  |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `role_relative`                                                | `bool`                                                    | `False`                                                                 | `True` / `False`                                                     | Player specific observation                                                                      | Renames observation depending on the role played by the agent. For example: if the agent is playing with `P1` role, `P1` key becomes `own` and `P2` key becomes `opp`, thus `obs["P1"]["key_1"]` becomes `obs["own"]["key_1"]` and `obs["P2"]["key_1"]` becomes `obs["opp"]["key_1"]` |

```python
wrappers_settings.role_relative = True
```

#### Flatten and Filter Observation

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                  |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `flatten`                                                | `bool`                                                    | `False`                                                                 | `True` / `False`                                                     | Observation                                                                      | Activates observation dictionary flattening, removing nested dictionaries and creating new keys joining original ones using "`_`" across nesting levels. For example: `obs["P1"]["key_1"]` becomes `obs["P1_key_1"]` |
| `filter_keys`                                            | `list` of `str`                                           | []                                                                  | -                                                                | Observation except `frame`                                                                      | Defines the list of RAM states to keep in the observation space                                                                                                                                                   |

```python
wrappers_settings.flatten = True
wrappers_settings.filter_keys = ["stage", "P1_health", "P2_char"]
```

{{% notice note %}}
The two operations are applied simultaneously, thus for filtering to be applied, flattening must be activated.
{{% /notice %}}

### Action Wrappers

#### Repeat Action

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `repeat_action`                                         | `int`                                                     | 1                                                                     | [1,&#160;12]                                                     | Keeps repeating the same action for _N_ environment steps    |

{{% notice note %}}
In order to use this wrapper, the `step_ratio` <a href="../envs/#settings">setting</a> must be set to equal to 1.
{{% /notice %}}

```python
wrappers_settings.repeat_action = 4
```

### Reward Wrappers

#### Normalize Reward

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `normalize_reward`                                   | `bool`                                                    | `False`                                                                 | `True` / `False`                                                     | Activates reward normalization. Divides reward value by the product between the `normalization_factor` value (see next row) and the delta between maximum and minium health bar value for the given game |
| `normalization_factor`                            | `float`                                                   | 0.5                                                                   | [-inf,&#160;+inf]                                                | Defines the normalization factor multiplying the delta between maximum and minium health bar value for the given game                                                                                           |

```python
wrappers_settings.normalize_reward = True
wrappers_settings.normalization_factor = 0.5
```

#### Clip Reward

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>         |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------ |
| `clip_reward`                                           | `bool`                                                    | `False`                                                                 | `True` / `False`                                                     | Activates reward clipping. Applies the sign function to the reward value |

```python
wrappers_settings.clip_reward = True
```

### Custom Wrappers

| <strong><span style="color:#5B5B60;">Name</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>         |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------ |
| `wrappers`                                           | `list` of pairs (`list`) of [`WrapperClass`, `kwargs`]                                                   | []                                                                 | -                                                     | Applies in sequence the list of wrappers classes with their correspondend settings |

```python
wrappers_settings.wrappers = \
   [[CustomWrapper1, {"setting1_1": value1_1,"setting1_2": value1_2}], \
    [CustomWrapper2, {"setting2_1": value2_1,"setting2_2": value2_2}]]
```

{{% notice note %}}
The custom wrappers are applied after all the activated built-in ones, if any.
{{% /notice %}}