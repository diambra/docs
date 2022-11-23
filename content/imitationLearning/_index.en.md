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

In order to activate the experience recording wrapper, one has just to add an additional kwargs dictionary, here named `traj_rec_settings`, to the environment creation method, as shown in the next code block. The dictionary has to be populated as described below.

```python
env = diambra.arena.make("doapp", settings, wrappers_settings, traj_rec_settings)
```

{{% notice note %}}
Implementation examples and templates can be found in the code repository, <a href="https://github.com/diambra/arena/tree/main/diambra/arena/wrappers" target="_blank">here</a>.
{{% /notice %}}

{{% notice tip %}}
Use of this functionality can be found in <a href="../gettingstarted/examples/humanexperiencerecorder/">this</a> example.
{{% /notice %}}

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                             |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `username`                                               | `string`                                                  | -                                                                     | -                                                                | Provides an identifier to be associated with the recorded trajectory                                                                                         |
| `file_path`                                              | `string`                                                  | -                                                                     | -                                                                | Specifies the path where to save recorded experiences                                                                                                        |
| `ignore_p2`                                              | `int`                                                     | -                                                                     | [0,&#160;1]                                                      | Specifies if to ignore P2 experience. Useful for example when recording expert demonstrations of a human player (P1) who is playing against an RL agent (P2) |

```python
trajRecSettings["username"] = "user"
trajRecSettings["file_path"] = "/home/user/DIAMBRA/"
trajRecSettings["ignore_p2"] = 0
```

{{% notice note %}}
In order to use this wrapper, the <a href="../wrappers/#flattening-and-filtering">Flattening and Filtering</a> wrapper must be disabled.
{{% /notice %}}

### Recorded Experience Loader

DIAMBRA Arena provides a dedicated class to load and use recorded trajectories for training. It needs the settings described in the following table in the form of a kwargs python dictionary. It also supports parallel environment executions, providing interfaces to easily integrate it with third party libraries.

{{% notice tip %}}
Use of this functionality can be found in <a href="../gettingstarted/examples/imitationlearning/">this</a> example.
{{% /notice %}}

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Value Range</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                          |
| -------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `traj_files_list`                                        | `string`                                                  | -                                                                     | -                                                                | Contains the list of recorded experience files, specified as absolute paths                               |
| `total_cpus`                                             | `int`                                                     | 1                                                                     | [1, inf)                                                         | Specifies the number of parallel environments one wants to run at the same time                           |
| `rank`                                                   | `int`                                                     | 0                                                                     | [0,&#160;settings["total_cpus"]-1]                               | Assigns a rank number to the environment to identify the instance number when using parallel environments |

```python
settings["traj_files_list"] = recorded_trajectories_files
settings["total_cpus"] = 1
settings["rank"] = 0
```

Once settings dictionary has been properly setup, the environment is created as shown in the next code block:

```python
env = diambra.arena.ImitationLearning(**settings)
```

The interaction with the environment follows the usual methods and conventions, except for the fact that actions in the step method are obviously ignored and the recorded ones are used.
