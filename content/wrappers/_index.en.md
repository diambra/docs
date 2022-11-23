---
title: Wrappers
weight: 40
---

### Index

<div style="font-size:1.125rem;">

- <a href="./#generic-wrappers">Generic Wrappers</a>
  - <a href="./#noop-steps-after-reset">NoOp Steps After Reset</a>
- <a href="./#observation-wrappers">Observation Wrappers</a>
  - <a href="./#frame-warping">Frame Warping</a>
  - <a href="./#frame-stacking-with-optional-dilation">Frame Stacking With Optional Dilation</a>
  - <a href="./#action-stacking">Action Stacking</a>
  - <a href="./#scaling">Scaling</a>
  - <a href="./#flattening-and-filtering">Flattening and Filtering</a>
- <a href="./#action-wrappers">Action Wrappers</a>
  - <a href="./#actions-sticking">Actions Sticking</a>
- <a href="./#reward-wrappers">Reward Wrappers</a>
  - <a href="./#reward-normalization">Reward Normalization</a>
  - <a href="./#reward-clipping">Reward Clipping</a>

</div>

DIAMBRA Arena comes with a large number of ready-to-use wrappers and examples showing how to apply them. They cover a wide spectrum of use cases, and also provide reference templates to develop custom ones. In order to activate wrappers, one has just to add an additional kwargs dictionary, here named `wrappers_settings`, to the environment creation method, as shown in the next code block. The dictionary has to be populated as described in the next sections.

```python
env = diambra.arena.make("doapp", settings, wrappers_settings)
```

{{% notice note %}}
Implementation examples and templates can be found in the code repository, <a href="https://github.com/diambra/arena/tree/main/diambra/arena/wrappers" target="_blank">here</a>.
{{% /notice %}}

{{% notice tip %}}
Use of these functionalities can be found in <a href="../gettingstarted/examples/wrappersoptions/">this</a> and <a href="../gettingstarted/examples/humanexperiencerecorder/">this</a> examples.
{{% /notice %}}

### Generic Wrappers

#### NoOp Steps After Reset

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>  |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| `no_op_max`                                              | `int`                                                     | 0                                                                     | [0,&#160;12]                                                     | Performs a maximum of _value_ No-Action steps after episode reset |

```python
wrappers_settings["no_op_max"] = 0
```

### Observation Wrappers

#### Frame Warping

{{% notice warning %}}
DEPRECATED: For speed, consider using the environment setting `frame_shape` that performs the same operation on the engine side, as described in <a href="/envs/#general-environment-settings">this section</a>.
{{% /notice %}}

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                    |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `hwc_obs_resize`                                         | `tuple` of three `int` (H,&#160;W,&#160;C)                | (84,&#160;84,&#160;0)                                                 | H,&#160;W:&#160;[1,&#160;512]<br>C:&#160;0, 1 or 3               | Frame                                                                           | Warps the frame from original Game resolution to H&#160;X&#160;W size.<br>C values:<br>0 - Deactivated;<br>1 - Grayscale;<br>3 - RGB;<br>Keeps observation element of type _Box_, changes its shape |

```python
wrappers_settings["hwc_obs_resize"] = (128, 128, 1)
```

#### Frame Stacking With Optional Dilation

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                               |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `frame_stack`                                            | `int`                                                     | 1                                                                     | [1,&#160;48]                                                     | Frame                                                                           | Stacks latest _value_ frames together along the third dimension.<br>Keeps observation element of type _Box_, changes its shape |
| `dilation`                                               | `int`                                                     | 1                                                                     | [1,&#160;48]                                                     | Frame                                                                           | Builds frame stacks adding one every _value_ frames.<br>Keeps observation element of type _Box_                                |

```python
wrappers_settings["frame_stack"] = 4
wrappers_settings["dilation"] = 1
```

#### Action Stacking

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                               |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `actions_stack`                                          | `int`                                                     | 1                                                                     | [1,&#160;48]                                                     | Actions-Move,<br>Actions-Attack                                                 | Stacks latest _value_ actions together for both moves and attacks.<br>Changes observation element type from _Discrete_ to _MultiDiscrete_, having _value_ elements, each with cardinality equal to the original _Discrete_ one |

```python
wrappers_settings["actions_stack"] = 12
```

#### Scaling

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `scale`                                                  | `bool`                                                    | False                                                                 | True / False                                                     | All                                                                             | Activates observation scaling.<br>Affects observation elements as follows:<br> - _Box_: type kept, changes bounds according to `scale_mod` setting.<br> - _Discrete (Binary)_: depends on `process_discrete_binary` (see below).<br> - _Discrete_: changed to _MultiBinary_ through one-hot encoding, size equal to original _Discrete_ cardinality.<br> - _MultiDiscrete_: changed to _N-MultiBinary_ through N-times one-hot encoding, N equal to original _MultiDiscrete_ size, encoding size equal to original _MultiDiscrete_ element cardinality |
| `exclude_image_scaling`                                  | `bool`                                                    | False                                                                 | True / False                                                     | Frame                                                                           | If True, prevents scaling to be applied on the game frame                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `process_discrete_binary`                                | `bool`                                                    | False                                                                 | True / False                                                     | Discrete Binary                                                                 | Controls how _Discrete (Binary)_ observations are treated: <br> - False: not affected; <br> - True: changed to _MultiBinary_ through one-hot encoding, size equal to 2                                                                                                                                                                                                                                                                                                                                                                                 |
| `scale_mod`                                              | `int`                                                     | 0                                                                     | [0,&#160;1]                                                      | All                                                                             | Defines the scaling bounds: 0 - [0,&#160;1]; 1 - [-1,&#160;1].<br>Keeps observation elements of the same type                                                                                                                                                                                                                                                                                                                                                                                                                                          |

```python
wrappers_settings["scale"] = True
wrappers_settings["exclude_image_scaling"] = True
wrappers_settings["process_discrete_binary"] = True
wrappers_settings["scale_mod"] = 0
```

#### Flattening and Filtering

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                  |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `flatten`                                                | `bool`                                                    | False                                                                 | True / False                                                     | RAM States                                                                      | Activates RAM States dictionary flattening, removing nested dictionaries and creating new keys joining original ones using "`_`" across nesting levels. For example: `obs["P1"]["key1"]` becomes `obs["P1_key1"]` |
| `filter_keys`                                            | `list` of `string`                                        | None                                                                  | -                                                                | RAM States                                                                      | Defines the list of RAM states to keep in the observation space                                                                                                                                                   |

```python
wrappers_settings["flatten"] = True
wrappers_settings["filter_keys"] = ["stage", "P1_ownHealth", "P1_oppChar"]
```

### Action Wrappers

#### Actions Sticking

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `sticky_actions`                                         | `int`                                                     | 1                                                                     | [1,&#160;12]                                                     | Keeps repeating the same action for _value_ environment steps    |

{{% notice note %}}
In order to use this wrapper, the `step_ratio` <a href="../envs/#settings">generic setting</a> must be set to equal to 1.
{{% /notice %}}

```python
wrappers_settings["sticky_actions"] = 1
```

### Reward Wrappers

#### Reward Normalization

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                                                                |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `reward_normalization`                                   | `bool`                                                    | False                                                                 | True / False                                                     | Activates reward normalization. Divides reward value by the product between the `reward_normalization_factor` (see next row) value and the delta between maximum and minium health bar value for the given game |
| `reward_normalization_factor`                            | `double`                                                  | 0.5                                                                   | [-inf,&#160;+inf]                                                | Defines the normalization factor multiplying the delta between maximum and minium health bar value for the given game                                                                                           |

```python
wrappers_settings["reward_normalization"] = True
wrappers_settings["reward_normalization_factor"] = 0.5
```

#### Reward Clipping

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>         |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------ |
| `clip_rewards`                                           | `bool`                                                    | False                                                                 | True / False                                                     | Activates reward clipping. Applies the sign function to the reward value |

```python
wrappers_settings["clip_rewards"] = False
```
