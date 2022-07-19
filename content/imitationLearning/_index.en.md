---
title: Imitation Learning
weight: 60
---

<div style="font-size:1.125rem;">

### Index

- <a href="./#experience-recording-wrapper">Experience Recording Wrapper</a>
- <a href="./#recorded-experience-loader">Recorded Experience Loader</a>

</div>

With the goal of easing the usage of Imitation Learning, DIAMBRA Arena comes with two, easy-to-use, useful features: an environment wrapper allowing to record agent experience (to be used, for example, to save human expert demonstrations), and a loader class allowing to flawlessly serve stored trajectories.

### Experience Recording Wrapper

In order to activate the experience recording wrapper, one has just to add an additional kwargs dictionary, here named `trajRecSettings`, to the environment creation method, as shown in the next code block. The dictionary has to be populated as described below.

```python
env = diambraArena.make("TestEnv", settings, wrappersSettings, trajRecSettings)

```

{{% notice note %}}
Implementation examples and templates can be found in the code repository, <a href="https://github.com/diambra/diambraArena/tree/main/diambraArena/wrappers" target="_blank">here</a>.
{{% /notice %}}

{{% notice tip %}}
Use of this functionality can be found in <a href="../gettingstarted/examples/humanexperiencerecorder/">this</a> example.
{{% /notice %}}

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|-----|
| `userName`     | `string`| - | - | Provides an identifier to be associated with the recorded trajectory  |
| `filePath`     | `string`| - | - | Specifies the path where to save recorded experiences |
| `ignoreP2`     | `int`| - | [0,&#160;1] | Specifies if to ignore P2 experience. Useful for example when recording expert demonstrations of a human player who is playing against an RL agent |

```python
trajRecSettings["userName"] = "Alex"
trajRecSettings["filePath"] = "/home/user/DIAMBRA/"
trajRecSettings["ignoreP2"] = 0
```

### Recorded Experience Loader

To load and use recorded trajectories for training, DIAMBRA Arena provides a dedicated class. It needs the settings described in the following table in the form of a kwargs python dictionary. It also supports parallel environment executions, providing interfaces to easily integrate it with third party libraries.

{{% notice tip %}}
Use of this functionality can be found in <a href="../gettingstarted/examples/imitationlearning/">this</a> example.
{{% /notice %}}

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
|-------------|-------------| ------|------|-----|
| `trajFilesList`     | `string`| - | - | Contains the list of recorded experience files, specified as absolute paths |
| `totalCpus`     | `int`| 1 | [1, inf) | Specifies the number of parallel environments one wants to run at the same time |
| `rank`     | `int`| 0 | [0,&#160;settings["totalCpus"]-1] | Assigns a rank number to the environment to identify the instance number when using parallel environments |

```python
settings["trajFilesList"] = recordedTrajectoriesFiles
settings["totalCpus"] = 1
settings["rank"] = 0
```
Once settings dictionary has been properly setup, the environment is created as shown in the next code block:

```python
env = diambraArena.imitationLearning(**settings)
```

The interaction with the environment follows the usual methods and conventions, except for the fact that actions in the step method are obviously ignored.
