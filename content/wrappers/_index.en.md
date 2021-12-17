---
title: Wrappers
weight: 40
---

### Index 

- <a href="/wrappers/#generic-wrappers" style="font-size:20px;">Generic Wrappers</a>
    - <a href="/wrappers/#noop-steps-after-reset" style="font-size:20px;">NoOp Steps After Reset</a>
- <a href="/wrappers/#observation-wrappers" style="font-size:20px;">Observation Wrappers</a>
    - <a href="/wrappers/#frame-warping" style="font-size:20px;">Frame Warping</a>
    - <a href="/wrappers/#frame-stacking-with-optional-dilation" style="font-size:20px;">Frame Stacking With Optional Dilation</a>
    - <a href="/wrappers/#action-stacking" style="font-size:20px;">Action Stacking</a>
    - <a href="/wrappers/#scaling" style="font-size:20px;">Scaling</a>
- <a href="/wrappers/#action-wrappers" style="font-size:20px;">Action Wrappers</a>
    - <a href="/wrappers/#action-sticking" style="font-size:20px;">Action Sticking</a>
- <a href="/wrappers/#reward-wrappers" style="font-size:20px;">Reward Wrappers</a>
    - <a href="/wrappers/#reward-normalization" style="font-size:20px;">Reward Normalization</a>
    - <a href="/wrappers/#reward-clipping" style="font-size:20px;">Reward Clippiing</a>

```python
# Gym wrappers settings
wrappersSettings = {}
```
### Generic Wrappers

#### NoOp Steps After Reset

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Note</span></strong> |
|-------------|-------------| ------|------|
| `noOpMax`     | `int`| [0,&#160;12]| Performs a maximum of *Value* NoOp steps after episode reset.<br>*0 by default.*  |

```python
wrappersSettings["noOpMax"] = 0 
```

### Observation Wrappers

#### Frame Warping

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>|<strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Note</span></strong> |
|-------------|-------------| ------|------|-------|
| `hwcObsResize`     | `list` of three `int` [H,&#160;W,&#160;C]| H,&#160;W:&#160;[1,&#160;512]<br>C:&#160;0, 1 or 3|Frame| Warps the frame from original Game resolution to H&#160;X&#160;W size.<br>C values: 0 - Deactivated; 1 - Grayscale; 3 - RGB;<br>Keeps observation element of type *Box*, changes its shape.<br>*Deactivated by default.*  |

```python
wrappersSettings["hwcObsResize"] = [128, 128, 1] 
```

#### Frame Stacking With Optional Dilation

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Target Observation Element</span></strong>| <strong><span style="color:#5B5B60;">Note</span></strong> |
|-------------|-------------| ------|------|------|
| `frameStack`     | `int` | [1,&#160;48] | Frame | Stacks last *Value* frames together along the third dimension.<br>Keeps observation element of type *Box*, changes its shape.<br>*1 by default.*  |
| `dilation`     | `int` | [1,&#160;48] | Frame | Builds frame stacks adding one every *Value* frames.<br>Keeps observation element of type *Box*.<br>*1 by default.*  |

```python
wrappersSettings["frameStack"] = 4
wrappersSettings["dilation"] = 1
```

#### Action Stacking

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Target Observation Element</span></strong>|<strong><span style="color:#5B5B60;">Note</span></strong> |
|-------------|-------------| ------|------|------|
| `actionsStack`   | `int` | [1,&#160;48] | Actions-Move,<br>Actions-Attack | Stacks last *Value* actions together for both moves and attacks.<br>Changes observation element type from *Discrete* to *MultiDiscrete*, having *Value* elements, each with cardinality equal to the original *Discrete* one.<br>*1 by default.*  |

```python
wrappersSettings["actionsStack"] = 12 
```

#### Scaling

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>|<strong><span style="color:#5B5B60;">Target Observation Element</span></strong> | <strong><span style="color:#5B5B60;">Note</span></strong> |
|-------------|-------------| ------|------|--------|
| `scale`     | `bool` | - | All | Activates observation scaling.<br>Affects observation elements as follows:<br> - *Box*: type kept, changes bounds according to `scaleMod` setting.<br> - *Discrete (Binary)*: not affected.<br> - *Discrete*: changed to *MultiDiscrete (Binary)* through one-hot encoding, size equal to original *Discrete* cardinality.<br> - *MultiDiscrete*: changed to *N-MultiDiscrete (Binary)* through N-times one-hot encoding, N equal to original *MultiDiscrete* size, encoding size equal to original *MultiDiscrete* element cardinality.<br>*False by default.*  |
| `scaleMod`     | `int` | [0,&#160;1] | All | Defines the scaling bounds: 0 - [0,&#160;1]; 1 - [-1,&#160;1].<br>Keeps observation elements of the same type.<br>*0 by default.*  |

```python
wrappersSettings["scale"] = True
wrappersSettings["scaleMod"] = 0
```

### Action Wrappers

#### Actions Sticking 

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Note</span></strong> |
|-------------|-------------| ------|------|
| `stickyActions` | `int`| [0,&#160;12]| Keeps repeating the same action for *Value* environment steps.<br>*1 by default.*  |

```python
wrappersSettings["stickyActions"] = 1
```

### Reward Wrappers

#### Reward Normalization

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Note</span></strong> |
|-------------|-------------| ------|------|
| `normalizeRewards` | `bool`| - | Activates reward normalization. Divides reward value by half the maximum health bar value.<br>*True by default.*  |

```python
wrappersSettings["normalizeRewards"] = True
```

#### Reward Clipping

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Note</span></strong> |
|-------------|-------------| ------|------|
| `clipRewards` | `bool`| - | Activates reward clipping. Applies the sign function to the reward value.<br>*False by default.*  |


```python
wrappersSettings["clipRewards"] = False
```
