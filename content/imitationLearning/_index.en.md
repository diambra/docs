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

In order to activate the experience recording wrapper, one has just to add an additional kwargs dictionary, here named `trajRecSettings`, to the environment creation method, as shown in the next code block. The dictionary has to be populated as described in the next sections.

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
| `userName`     | `string`| - | Provides an identifier to be associated with the recorded trajectory  |
| `filePath`     | `string`| - | Specifies the path where to save recorded experiences |
| `ignoreP2`     | `int`| [0,&#160;1] | Specifies if to ignore P2 experience. Useful for example when recording expert demonstrations of a human player who is playing against an RL agent |

```python                                                                       
trajRecSettings["userName"] = "Alex"                                            
trajRecSettings["filePath"] = "/home/user/DIAMBRA/"                    
trajRecSettings["ignoreP2"] = 0                                                 
```     

### Recorded Experience Loader

{{% notice tip %}}
A ready-to-use example showing how the recorded experience loader is set up and used can be found in the Examples folder inside DIAMBRA Arena repository, and described in <a href="../gettingstarted/examples/imitationlearning/">this section</a> of this manual.
{{% /notice %}}

```python                                                                       
# Show files in folder                                                          
basePath = os.path.dirname(os.path.abspath(__file__))                           
recordedTrajectoriesFolder = os.path.join(basePath, "recordedTrajectories")     
recordedTrajectoriesFiles = [os.path.join(recordedTrajectoriesFolder, f)        
                             for f in os.listdir(recordedTrajectoriesFolder)    
                             if os.path.isfile(os.path.join(recordedTrajectoriesFolder, f))]
print(recordedTrajectoriesFiles)                                                
                                                                                
# Imitation learning settings                                                   
settings = {}                                                                   
                                                                                
# List of recorded trajectories files                                           
settings["trajFilesList"] = recordedTrajectoriesFiles                           
                                                                                
# Number of parallel Imitation Learning environments to run                     
settings["totalCpus"] = 2                                                       
```                                                                             
                                                                                
#### Environment execution                                                      
                                                                                
```python                                                                       
env = diambraArena.imitationLearning(**settings)            
```
