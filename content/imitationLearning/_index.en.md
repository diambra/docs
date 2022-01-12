---
title: Imitation Learning
weight: 60
---

<div style="font-size:1.125rem;">

### Index

- <a href="./#experience-recording-wrapper">Experience Recording Wrapper</a>
- <a href="./#recorded-experience-loader">Recorded Experience Loader</a>

</div>

### Experience Recording Wrapper

```python
env = diambraArena.make("TestEnv", settings, wrappersSettings, trajRecSettings)

```

{{% notice note %}}
Implementation examples and templates can be found in the code repository, <a href="./#reward-clipping" target="_blank">here ADD LINK</a>
{{% /notice %}}

{{% notice tip %}}
A ready-to-use example showing how the recording wrapper is set up and used can be found in the Examples folder inside DIAMBRA Arena repository, and described in <a href="../gettingstarted/examples/humanexperiencerecorder/">this section</a> of this manual.
{{% /notice %}}

| <strong><span style="color:#5B5B60;">Key</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Value</span></strong>| <strong><span style="color:#5B5B60;">Note</span></strong> |
|-------------|-------------| ------|------|
| `noOpMax`     | `int`| [0,&#160;12]| Performs a maximum of *Value* NoOp steps after episode reset.<br>*0 by default.*  |

```python
# Gym trajectory recording wrapper kwargs
trajRecSettings = {}
homeDir = expanduser("~")

# Username
trajRecSettings["userName"] = "Alex"

# Path where to save recorderd trajectories
trajRecSettings["filePath"] = os.path.join(homeDir, "diambraArena/trajRecordings",
                                         settings["gameId"])

# If to ignore P2 trajectory (useful when collecting
# only human trajectories while playing as a human against a RL agent)
trajRecSettings["ignoreP2"] = 0
```


### Recorded Experience Loader

{{% notice tip %}}
A ready-to-use example showing how the recorded experience loader is set up and used can be found in the Examples folder inside DIAMBRA Arena repository, and described in <a href="../gettingstarted/examples/imitationlearning/">this section</a> of this manual.
{{% /notice %}}
