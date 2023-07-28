---
title: Stable Baselines 3
weight: 10
---

<div style="font-size:1.125rem;">

### Index

- <a href="./#getting-ready">Getting Ready</a>
  - <a href="./#native-interface">Native Interface</a>
- <a href="./#basic">Basic</a>
  - <a href="./#basic-example">Basic Example</a>
  - <a href="./#saving-loading-and-evaluating">Saving, Loading and Evaluating</a>
  - <a href="./#parallel-environments">Parallel Environments</a>
- <a href="./#advanced">Advanced</a>
  - <a href="./#dictionary-observations">Dictionary Observations</a>
  - <a href="./#complete-training-script">Complete Training Script</a>
  - <a href="./#agent-script-for-competition">Agent Script for Competition</a>

</div>

{{% notice tip %}}
The source code of all examples described in this section is available in our <a href="https://github.com/diambra/agents/tree/main/stable_baselines3" target="_blank">DIAMBRA Agents</a> repository.
{{% /notice %}}

### Getting Ready

We highly recommend using virtual environments to isolate your python installs, especially to avoid conflicts in dependencies. In what follows we use Conda but any other tool should work too.

Create and activate a new dedicated virtual environment:

```shell
conda create -n diambra-arena-sb3 python=3.8
conda activate diambra-arena-sb3
```

Install DIAMBRA Arena with Stable Baselines 3 interface:

```shell
pip install diambra-arena[stable-baselines3]
```

This should be enough to prepare your system to execute the following examples. You can refer to the official <a href="https://stable-baselines3.readthedocs.io/en/master/guide/install.html" target="_blank">Stable Baselines 3 documentation</a> or reach out on our <a href="https://diambra.ai/discord" target="_blank">Discord server</a> for specific needs.

All the examples presented below are available here: <a href="https://github.com/diambra/agents/tree/main/stable_baselines3" target="_blank">DIAMBRA Agents - Stable Baselines 3</a>. They have been created following the high level approach found on <a href="https://stable-baselines3.readthedocs.io/en/master/guide/examples.html" target="_blank">Stable Baselines 3 examples</a> page, thus allowing to easily extend them and to understand how they interface with the different components.

These examples only aims at demonstrating the core functionalities and high level aspects, they will not generate well performing agents, even if the training time is extended to cover a large number of training steps. The user will need to build upon them, exploring aspects like: policy network architecture, algorithm hyperparameter tuning, observation space tweaking, rewards wrapping and other similar ones.

#### Native interface

DIAMBRA Arena native interface with Stable Baselines 3 covers a wide range of use cases, automating handling of vectorized environments and monitoring wrappers. In the majority of cases it will be sufficient for users to directly import and use it, with no need for additional customization. Below is reported its interface and a table describing its arguments.

```python
def make_sb3_env(game_id: str, env_settings: dict={}, wrappers_settings: dict={},
                 use_subprocess: bool=True, seed: int=0, log_dir_base: str="/tmp/DIAMBRALog/",
                 start_index: int=0, allow_early_resets: bool=True,
                 start_method: str=None, no_vec: bool=False)
```

| <strong><span style="color:#5B5B60;">Argument</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                       |
| ------------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `game_id`                                                     | `str`                                                     | -                                                                     | Game environment identifier                                                                                                                                            |
| `env_settings`                                                | `dict`                                                    | `{}`                                                                  | Environment settings (<a href="/envs/#settings">see more</a>)                                                                                                          |
| `wrappers_settings`                                           | `dict`                                                    | `{}`                                                                  | Wrappers settings (<a href="/wrappers/">see more</a>)                                                                                                                  |
| `use_subprocess`                                              | `bool`                                                    | `True`                                                                | If to use subprocesses for multi-threaded parallelization                                                                                                              |
| `seed`                                                        | `int`                                                     | 0                                                                     | Random number generator seed                                                                                                                                           |
| `log_dir_base`                                                | `str`                                                     | `"/tmp/DIAMBRALog/"`                                                  | Folder where to save execution logs                                                                                                                                    |
| `start_index`                                                 | `int`                                                     | 0                                                                     | Starting process rank index                                                                                                                                            |
| `allow_early_resets`                                          | `bool`                                                    | `True`                                                                | Monitor wrapper argument to allow environment reset before it is done                                                                                                  |
| `start_method`                                                | `str`                                                     | `None`                                                                | Method to spawn subprocesses when active (<a href="https://stable-baselines3.readthedocs.io/en/master/guide/vec_envs.html#subprocvecenv" target="_blank">see more</a>) |
| `no_vec`                                                      | `bool`                                                    | `False`                                                               | If `True` avoids using vectorized environments (valid only when using a single instance)                                                                               |

{{% notice note %}}
For the interface low level details, users can review the correspondent source code <a href="https://github.com/diambra/arena/tree/main/diambra/arena/stable_baselines3" target="_blank">here</a>.
{{% /notice %}}

### Basic

For all the basic examples, the environment will be used in `hardcore` mode, so that the observation space will be only of type `Box` composed by screen pixels, as in the majority of simple examples found in tutorials and docs. This allows to directly use it without the need of further processing.

#### Basic Example

This example demonstrates how to:

- Instantiate a new DIAMBRA Arena environment with its settings
- Interface it with one of Stable Baselines 3's algorithms
- Train the algorithm
- Run the trained agent in the environment for one episode

It uses the A2C algorithm, with a `CnnPolicy` policy network to properly process the game frame observation as input. For demonstration purposes, the algorithm is trained for only 200 steps, so the resulting agent will be far from optimal.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/stable_baselines3/basic.py" >}}

How to run it:

```shell
diambra run python basic.py
```

#### Saving, loading and evaluating

In addition to what seen in the previous example, this one demonstrates how to:

- Save a trained agent
- Load a saved agent
- Evaluate an agent on a given number of episodes

The same conditions of the previous example for algorithm, policy and training steps are used in this one too.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/stable_baselines3/saving_loading_evaluating.py" >}}

How to run it:

```shell
diambra run python saving_loading_evaluating.py
```

#### Parallel Environments

In addition to what seen in previous examples, this one demonstrates how to:

- Leverage DIAMBRA Arena native Stable Baselines 3 interface
- Activate environment wrappers
- Run training using parallel environments
- Print out the policy network architecture

In this example, the PPO algorithm is used, with the same `CnnPolicy` seen before. This policy network works even if in this example an environment wrapper is used to stack multiple game frames, as they are piled along the channel dimension. In this example the policy architecture is also printed to the console output, allowing to visualize how inputs are processed and "translated" to actions probabilities.

This example also runs multiple environments, automatically detecting the number of instances created by DIAMBRA CLI when running the script.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/stable_baselines3/parallel_envs.py" >}}

How to run it:

```shell
diambra run -s=2 python parallel_envs.py
```

### Advanced

The nex examples make use of the complete observation space of our environments. This is of type `Dict`, in which different elements are organized as key-value pairs and they can be of different type.

#### Dictionary Observations

In addition to what seen in previous examples, this one demonstrates how to:

- Activate a complete set of environment wrappers
- How to properly handle dictionary observations for Stable Baselines 3

There are two main things to note in this example: how to handle observation normalization and dictionary observations. As it can be seen from the snippet below, the normalization wrapper is applied on all elements but the image frame, as Stable Baselines 3 automatically normalizes images and expects their pixels to be in the range [0 - 255]. The library also has a specific constraint on dictionary observation spaces: they cannot be nested. For this reason we provide a flattening wrapper that creates a shallow, not nested, dictionary from the original observation space, allowing in addition to filter it by keys.

In this case, the policy network needs to be of class `MultiInputPolicy`, since it will handle different types of inputs. Stable Baselines 3 automatically defines the network architecture, properly matching the input type. The architecture is then printed to the console output, allowing to clearly identify all the different contributions.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/stable_baselines3/dict_obs_space.py" >}}

How to run it:

```shell
diambra run python dict_obs_space.py
```

#### Complete Training Script

In addition to what seen in previous examples, this one demonstrates how to:

- Build a complete training script to be used with Stable Baselines via a config fila
- How to properly handle hyper-parameters scheduling via callbacks
- How to use callbacks for auto-saving
- How to control some policy network models and optimizer parameters

This example show exactly how we trained our own models on these environments. It should be considered a starting point from where to explore and experiment, the following are just a few options among the most obvious ones:

- Tweak hyper-parameters for the chosen algorithm
- Evolve the policy network architecture
- Test different algorithms, both on and off-policy
- Try to leverage behavioral cloning / imitation learning
- Modify the reward function to guide learning in other directions

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/stable_baselines3/training.py" >}}

How to run it:

```shell
diambra run python training.py --cfgFile /absolute/path/to/config.yaml
```

and the configuration file to be used with this training script is reported below:

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/stable_baselines3/cfg_files/doapp/sr6_128x4_das_nc.yaml" >}}

#### Agent Script for Competition

Finally, after the agent training is completed, besides running it locally in your own machine, you may want to submit it to our competition platform! To do so, you can use the following script that provides a ready to use, flexible example that can accommodate different models, games and settings.

{{% notice tip %}}
To submit your trained agent to our platform, compete for the first leaderboard positions, and unlock our achievements, follow the simple steps described in the <a href="/competitionplatform/howtosubmitanagent/">"How to Submit an Agent"</a> section.
{{% /notice %}}

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/stable_baselines3/agent.py" >}}

How to run it locally:

```shell
diambra run python agent.py --cfgFile /absolute/path/to/config.yaml --trainedModel "model_name"
```

and the configuration file to be used is the same that was used for training it, like the one reported in the previous paragraph.
