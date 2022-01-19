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
- <a href="./#action-wrappers">Action Wrappers</a>
    - <a href="./#actions-sticking">Actions Sticking</a>
- <a href="./#reward-wrappers">Reward Wrappers</a>
    - <a href="./#reward-normalization">Reward Normalization</a>
    - <a href="./#reward-clipping">Reward Clipping</a>

</div>


DIAMBRA Arena comes with a large number of ready-to-use wrappers and examples showing how to apply them. They cover a wide spectrum of use cases, and also provide reference templates to develop custom ones. In order to activate wrappers, one has just to add an additional kwargs dictionary, here named `wrappersSettings`, to the environment creation method, as shown in the next code block. The dictionary has to be populated as described in the next sections.

```python
env = diambraArena.make("TestEnv", settings, wrappersSettings)
```

{{% notice note %}}
Implementation examples and templates can be found in the code repository, <a href="./#reward-clipping" target="_blank">here ADD LINK</a>
{{% /notice %}}

{{% notice tip %}}
Use of these functionalities can be found in <a href="../gettingstarted/examples/wrappersoptions/">this</a> and <a href="../gettingstarted/examples/humanexperiencerecorder/">this</a> examples.
{{% /notice %}}

### Generic Wrappers

#### NoOp Steps After Reset

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> |<strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|-----|
| `noOpMax`     | `int`| 0 | [0,&#160;12]| Performs a maximum of *Value* NoOp steps after episode reset. |

```python
wrappersSettings["noOpMax"] = 0 
```

### Observation Wrappers

#### Frame Warping

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong>|<strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|-------|-----|
| `hwcObsResize`     | `list` of three `int` [H,&#160;W,&#160;C]| [84,&#160;84,&#160;0] | H,&#160;W:&#160;[1,&#160;512]<br>C:&#160;0, 1 or 3|Frame| Warps the frame from original Game resolution to H&#160;X&#160;W size.<br>C values:<br>0 - Deactivated;<br>1 - Grayscale;<br>3 - RGB;<br>Keeps observation element of type *Box*, changes its shape.  |

```python
wrappersSettings["hwcObsResize"] = [128, 128, 1] 
```

#### Frame Stacking With Optional Dilation

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> |<strong><span style="color:#5B5B60;">Target Observation Element</span></strong>| <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|------|------|
| `frameStack`     | `int` | 1 |[1,&#160;48] | Frame | Stacks last *Value* frames together along the third dimension.<br>Keeps observation element of type *Box*, changes its shape. |
| `dilation`     | `int` | 1 | [1,&#160;48] | Frame | Builds frame stacks adding one every *Value* frames.<br>Keeps observation element of type *Box*. |

```python
wrappersSettings["frameStack"] = 4
wrappersSettings["dilation"] = 1
```

#### Action Stacking

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Target Observation Element</span></strong>|<strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|------|----|
| `actionsStack`   | `int` | 1|[1,&#160;48] | Actions-Move,<br>Actions-Attack | Stacks last *Value* actions together for both moves and attacks.<br>Changes observation element type from *Discrete* to *MultiDiscrete*, having *Value* elements, each with cardinality equal to the original *Discrete* one. |

```python
wrappersSettings["actionsStack"] = 12 
```

#### Scaling

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> |<strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|--------|------|
| `scale`     | `bool` | False |True / False | All | Activates observation scaling.<br>Affects observation elements as follows:<br> - *Box*: type kept, changes bounds according to `scaleMod` setting.<br> - *Discrete (Binary)*: not affected.<br> - *Discrete*: changed to *MultiDiscrete (Binary)* through one-hot encoding, size equal to original *Discrete* cardinality.<br> - *MultiDiscrete*: changed to *N-MultiDiscrete (Binary)* through N-times one-hot encoding, N equal to original *MultiDiscrete* size, encoding size equal to original *MultiDiscrete* element cardinality.|
| `scaleMod`     | `int` | 0|[0,&#160;1] | All | Defines the scaling bounds: 0 - [0,&#160;1]; 1 - [-1,&#160;1].<br>Keeps observation elements of the same type.|

```python
wrappersSettings["scale"] = True
wrappersSettings["scaleMod"] = 0
```

### Action Wrappers

#### Actions Sticking 

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> |<strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|------|
| `stickyActions` | `int`| 1 | [1,&#160;12]| Keeps repeating the same action for *Value* environment steps.  |

```python
wrappersSettings["stickyActions"] = 1
```

### Reward Wrappers

#### Reward Normalization

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> |<strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|-----|
| `normalizeRewards` | `bool`| True|True / False | Activates reward normalization. Divides reward value by half the maximum health bar value.  |

```python
wrappersSettings["normalizeRewards"] = True
```

#### Reward Clipping

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong>|<strong><span style="color:#5B5B60;">Value Range</span></strong> |<strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|-------|
| `clipRewards` | `bool`| False | True / False | Activates reward clipping. Applies the sign function to the reward value. |


```python
wrappersSettings["clipRewards"] = False
```
