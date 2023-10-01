---
title: Ray RLlib
weight: 20
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
  - <a href="./#agent-script-for-competition">Agent Script for Competition</a>

</div>

{{% notice tip %}}
The source code of all examples described in this section is available in our <a href="https://github.com/diambra/agents/tree/main/ray_rllib" target="_blank">DIAMBRA Agents</a> repository.
{{% /notice %}}

### Getting Ready

We highly recommend using virtual environments to isolate your python installs, especially to avoid conflicts in dependencies. In what follows we use Conda but any other tool should work too.

Create and activate a new dedicated virtual environment:

```shell
conda create -n diambra-arena-ray python=3.8
conda activate diambra-arena-ray
```

Install DIAMBRA Arena with Ray RLlib interface:

```shell
pip install diambra-arena[ray-rllib]
```

This should be enough to prepare your system to execute the following examples. You can refer to the official <a href="https://docs.ray.io/en/latest/rllib/index.html" target="_blank">Ray RLlib documentation</a> or reach out on our <a href="https://diambra.ai/discord" target="_blank">Discord server</a> for specific needs.

All the examples presented below are available here: <a href="https://github.com/diambra/agents/tree/main/ray_rllib" target="_blank">DIAMBRA Agents - Ray RLlib.</a> They have been created following the high level approach found on <a href="https://docs.ray.io/en/latest/rllib/rllib-examples.html" target="_blank">Ray RLlib examples</a> page and their related <a href="https://github.com/ray-project/ray/tree/master/rllib/examples" target="_blank">repository collection,</a> thus allowing to easily extend them and to understand how they interface with the different components.

These examples only aims at demonstrating the core functionalities and high level aspects, they will not generate well performing agents, even if the training time is extended to cover a large number of training steps. The user will need to build upon them, exploring aspects like: policy network architecture, algorithm hyperparameter tuning, observation space tweaking, rewards wrapping and other similar ones.

#### Native interface

DIAMBRA Arena native interface with Ray RLlib covers a wide range of use cases, automating handling of key things like parallelization. In the majority of cases it will be sufficient for users to directly import and use it, with no need for additional customization.

{{% notice note %}}
For the interface low level details, users can review the correspondent source code <a href="https://github.com/diambra/arena/blob/main/diambra/arena/ray_rllib/" target="_blank">here</a>.
{{% /notice %}}

### Basic

#### Basic Example

This example demonstrates how to:

- Build the config dictionary for Ray RLlib
- Interface one of Ray RLlib's algorithms with DIAMBRA Arena using the native interface
- Train the algorithm
- Run the trained agent in the environment for one episode

It uses the PPO algorithm and, for demonstration purposes, the algorithm is trained for only 200 steps, so the resulting agent will be far from optimal.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/ray_rllib/basic.py" >}}

How to run it:

```shell
diambra run python basic.py
```

#### Saving, loading and evaluating

In addition to what seen in the previous example, this one demonstrates how to:

- Print out the policy network architecture
- Save a trained agent
- Load a saved agent
- Evaluate an agent on a given number of episodes
- Print training and evaluation results

The same conditions of the previous example for algorithm, policy and training steps are used in this one too.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/ray_rllib/saving_loading_evaluating.py" >}}

How to run it:

```shell
diambra run python saving_loading_evaluating.py
```

#### Parallel Environments

In addition to what seen in previous examples, this one demonstrates how to:

- Run training and evaluation using parallel environments

This example runs multiple environments. In order to properly execute it, the user needs to specify the correct number of environments instances to be created via DIAMBRA CLI when running the script. In particular, in this case, 6 different instances are needed:

- 2 rollout workers with 2 environments each, accounting for 4 environments
- 1 evaluation worker with 2 environments, accounting for the remaining 2 environments

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/ray_rllib/parallel_envs.py" >}}

How to run it:

```shell
diambra run -s=6 python parallel_envs.py
```

### Advanced

#### Dictionary Observations

In addition to what seen in previous examples, this one demonstrates how to:

- Activate a complete set of environment wrappers
- How to properly handle dictionary observations for Ray RLlib

The main thing to note in this example is that the library does not have constraints on dictionary observation spaces, being able to handle nested ones too.

The policy network is automatically generated, properly handling different types of inputs. Model architecture is then printed to the console output, allowing to clearly identify all the different contributions.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/ray_rllib/dict_obs_space.py" >}}

How to run it:

```shell
diambra run python dict_obs_space.py
```

#### Agent Script for Competition

Finally, after the agent training is completed, besides running it locally in your own machine, you may want to submit it to our <a href="../../competitionplatform">Competition Platform!</a> To do so, you can use the following script that provides a ready to use, flexible example that can accommodate different models, games and settings.

{{% notice tip %}}
To submit your trained agent to our platform, compete for the first leaderboard positions, and unlock our achievements, follow the simple steps described in the <a href="../../competitionplatform/howtosubmitanagent/">"How to Submit an Agent"</a> section.
{{% /notice %}}

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/ray_rllib/agent.py" >}}

How to run it locally:

```shell
diambra run python agent.py --trainedModel /absolute/path/to/checkpoint/ --envSpaces /absolute/path/to/environment/spaces/descriptor/
```