---
title: SheepRL
weight: 5
---

<div style="font-size:1.125rem;">

### Index

- <a href="#getting-ready">Getting Ready</a>
  - <a href="#general-environment-settings">General Environment Settings</a>
  - <a href="#native-interface">Native Interface</a>
  - <a href="#agent-settings">Agent Settings</a>
- <a href="#basic">Basic</a>
  - <a href="#customising-the-configurations">Customising the Configurations</a>
  - <a href="#basic-example">Basic Example</a>
    - <a href="#configs-folder">Configs Folder</a>
    - <a href="#define-the-environment">Define the Environment</a>
    - <a href="#define-the-agent">Define the Agent</a>
    - <a href="#define-the-experiment">Define the Experiment</a>
    - <a href="#train-and-evaluate-the-agent">Train and Evaluate the Agent</a>
    - <a href="#train-and-evaluate-scripts">Train and Evaluate Scripts</a>
    - <a href="#ppo-implementation">PPO Implementation</a>
  - <a href="#parallel-environments">Parallel Environments</a>
- <a href="#advanced">Advanced</a>
  - <a href="#fabric">Fabric</a>
  - <a href="#metric-and-logging">Metric and Logging</a>
  - <a href="#agent-script-for-competition">Agent Script for Competition</a>

</div>

{{% notice tip %}}
The source code of all examples described in this section is available in our <a href="https://github.com/diambra/agents/tree/main/sheeprl" target="_blank">DIAMBRA Agents</a> repository.
{{% /notice %}}

### Getting Ready

We highly recommend using virtual environments to isolate your python installs, especially to avoid conflicts in dependencies. In what follows we use Conda but any other tool should work too.

Create and activate a new dedicated virtual environment:

```shell
conda create -n diambra-arena-sheeprl python=3.9
conda activate diambra-arena-sheeprl
```

Install DIAMBRA Arena with SheepRL interface:

```shell
pip install diambra-arena[sheeprl]
```

This should be enough to prepare your system to execute the following examples. You can refer to the official <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/v0.5.5" target="_blank">SheepRL documentation</a> or reach out on our <a href="https://diambra.ai/discord" target="_blank">Discord server</a> for specific needs.

{{% notice warning %}}
Remember that to train agents, you must have installed the `diambra` CLI (`python3 -m pip install diambra`) and set the `DIAMBRAROMSPATH` environment variable properly.
{{% /notice %}}

All the examples presented below are available here: <a href="https://github.com/diambra/agents/tree/main/sheeprl" target="_blank">DIAMBRA Agents - SheepRL</a>. They have been created following the high level approach found on <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/v0.5.5/howto/learn_in_diambra.md" target="_blank">SheepRL DIAMBRA</a> page, thus allowing to easily extend them and to understand how they interface with the different components.

These examples only aim at demonstrating the core functionalities and high-level aspects, they will not generate well-performing agents, even if the training time is extended to cover a large number of training steps. The user will need to build upon them, exploring aspects like policy network architecture, algorithm hyperparameter tuning, observation space tweaking, rewards wrapping, and other similar ones.

#### General Environment Settings
SheepRL provides a lot of different environments that share a set of parameters. Moreover, SheepRL leverages <a href="https://hydra.cc/" target="_blank">Hydra</a> for defining hierarchical configurations. Below is reported the general structure of the configuration of an environment and a table describing the arguments.

{{< github_code "https://raw.githubusercontent.com/Eclectic-Sheep/sheeprl/v0.5.5/sheeprl/configs/env/default.yaml" >}}

| <strong><span style="color:#5B5B60;">Argument</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                       |
| ------------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`  | `str` | - | Game environment identifier |
| `num_envs` | `int` | `4` | The number of environment to initialize for training |
| `frame_stack` | `int` | `1` | The number of frames to stack |
| `sync_env` | `bool` | `False` | Whether to use the `gymnasium.vector.SyncVectorEnv` (`True`) or `gymnasium.vector.AsyncVectorEnv` (`False`) for handling vectorized environments |
| `screen_size` | `int \| Tuple[int, int]` | `64` | Screen size of the frames |
| `action_repeat` | `int` | `64` | How many times repeat the same action |
| `grayscale` | `bool` | `False` | Whether to use grayscale frames |
| `clip_rewards` | `bool` | `False` | Whether or not to clip rewards using a `tanh` |
| `capture_video` | `bool` | `True` | Whether or not to capture the video of the episodes during training |
| `frame_stack_dilation` | `int` | `1` | The number of frames to be skipped between frames in the `frame_stack` |
| `max_episode_steps` | `int \| None` | `null` | The maximum number of steps in a single episode |
| `reward_as_observation` | `bool` | `False` | Whether or not to add the reward to the observations |
| `wrapper` | `Dict[str, Any]` | - | Environment-related arguments (see <a href="#native-interface">here</a>) |


{{% notice note %}}
If you have never used Hydra, before continuing, it is strongly recommended to check the <a href="https://hydra.cc/" target="_blank">Hydra official documentation</a> and the <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/v0.5.5/howto/configs.md" target="_blank">SheepRL-related section</a>.
{{% /notice %}}


#### Native interface

DIAMBRA Arena native interface with SheepRL covers a wide range of use cases, automating the handling of vectorized environments and monitoring wrappers. In the majority of cases, it will be sufficient for users to directly import and use it, with no need for additional customization. Below is reported its interface and a table describing its arguments.

```python
class DiambraWrapper(gym.Wrapper):
    def __init__(
        self,
        id: str,
        action_space: str = "DISCRETE",
        screen_size: Union[int, Tuple[int, int]] = 64,
        grayscale: bool = False,
        repeat_action: int = 1,
        rank: int = 0,
        diambra_settings: Dict[str, Any] = {},
        diambra_wrappers: Dict[str, Any] = {},
        render_mode: str = "rgb_array",
        log_level: int = 0,
        increase_performance: bool = True,
    ):
```

| <strong><span style="color:#5B5B60;">Argument</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                       |
| ------------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`  | `str` | - | Game environment identifier |
| `action_space` | `str` | `"DISCRETE"` | Which action space to use: one between `"DISCRETE"` and `"MULTI_DISCRETE"` |
| `screen_size` | `int \| Tuple[int, int]` | `64` | Screen size of the frames |
| `grayscale` | `bool` | `False` | Whether to use grayscale frames |
| `rank` | `int` | `0` | Rank of the environment |
| `diambra_settings` | `Dict[str, Any]` | `{}` | The settings of the environment. See <a href="../../envs/#settings" target="_blank">here</a> to check which settings you can specify. |
| `diambra_wrappers` | `Dict[str, Any]` | `{}` | The wrappers to apply to the environment. See <a href="../../wrappers/" target="_blank">here</a> to check which wrappers you can specify. |
| `render_mode` | `str` | `"rgb_array"` | Rendering mode |
| `log_level` | `int` | `0` | Log level |
| `increase_performance` | `bool` | `True` | Whether to modify frames on the engine side (`True`) or use the wrapper (`False`) |

{{% notice note %}}
For the interface low-level details, users can review the correspondent source code <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/v0.5.5/sheeprl/envs/diambra.py" target="_blank">here</a>.
{{% /notice %}}


#### Agent Settings
SheepRL provides several SOTA algorithms, both model-free and model-based. <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/v0.5.5/sheeprl/configs/algo" target="_blank">Here</a> you can find the default configurations for these agent. Of course, one can change algorithm-related hyper-parameters for customizing his/her experiments.

### Basic
As anticipated before, SheepRL provides several default configurations for all its components, which are available and can be composed to set up an experiment. Otherwise, you can customize the ones you want: the two main ones to be defined for experiments are the agent and the environment. 

Regarding the environment, there are some constraints that must be respected, for example, the dictionary observation spaces cannot be nested. For this reason, the DIAMBRA <a href="../../wrappers/#flatten-and-filter-observation" target="_blank">flattening wrapper</a> is always used. For more information about the constraints of the SheepRL library, check <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/v0.5.5/howto/learn_in_diambra.md#args" target="_blank">here</a>.

Instead, regarding the agent, the only two constraints that are present concern the observation and action spaces that agents support. You can read the supported observation and action spaces in Table 1 and Table 2 of the <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/v0.5.5" target="_blank">README</a> in the SheepRL GitHub repository, respectively.


#### Customising the Configurations
The default configurations are available <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/v0.5.5/sheeprl/configs" target="_blank">here</a>. If you want to define your custom experiments, you just need to follow a few steps:
1. You need to create a folder (with the same structure as the <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/v0.5.5/sheeprl/configs" target="_blank">SheepRL configs folder</a>) where to place your custom configurations. 
2. You need to define the `SHEEPRL_SEARCH_PATH` environment variable in the `.env` file as follows: `SHEEPRL_SEARCH_PATH=file://relative/path/to/custom/configs/folder;pkg://sheeprl.configs`.
3. You need to define the custom configurations, being careful that the filename is different from the default ones. If this is not respected, your file will overwrite the default configurations.

#### Basic Example

This example demonstrates how to:

* Leverage SheepRL to define the environment for training.
* Define a PPO Agent to be trained.
* Define custom configurations for your experiment.
* Train the agent.
* Run the trained agent in the environment for one episode.


SheepRL natively supports dictionary observation spaces, the only thing you need to define is the keys of the observations you want to process. For more information about observations selection, check <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/v0.5.5/howto/select_observations.md" target="_blank">here</a>.

##### Configs Folder
First, it is necessary to create a folder for the configuration files. We create the `configs` folder under the `./sheeprl/` folder in the <a href="https://github.com/diambra/agents/tree/main" target="_blank">DIAMBRA Arena</a> GitHub repository. Then we added the `.env` file in `./sheeprl/` folder, in which we need to define the `SHEEPRL_SEARCH_PATH` environment variable as follows:

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/.env" >}}

##### Define the Environment
Now, in the `./sheeprl/configs` folder we create the `env` folder in which the `custom_env.yaml` will be placed.
Below is reported a possible configuration of the environment.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/configs/env/custom_env.yaml" >}}

##### Define the Agent
As for the environment, we need to create a dedicated folder to place the custom configurations of the agents: we create the `algo` folder in the `./sheeprl/configs` folder and we place the `custom_ppo_agent.yaml` file. Under the `default` keyword, it is possible to retrieve the configurations specified in another file, in our case, since we are defining the agent, we can take the configuration from the <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/v0.5.5/sheeprl/configs/algo" target="_blank">algorithm config folder</a> in SheepRL, in which several SOTA agents are defined.

{{% notice note %}}
When defining an agent it is mandatory to define the `name` of the algorithm (it must be equal to the filename of the file in which the algorithm is defined). The value of these parameters defines which algorithm will be used for training. If you inherit the default configurations of a specific algorithm, then you do not need to define it, since it is already defined in the default configs of that algorithm.
{{% /notice %}}

Below is reported a configuration file for a PPO agent.
{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/configs/algo/custom_ppo_agent.yaml" >}}

##### Define the Experiment
The last thing to do is to define the experiment. You just need to define a `custom_exp.yaml` file in the `./sheeprl/configs/exp` folder and assemble the environment, the agent, and the other components of the SheepRL framework. In particular, there are four parameters that must be defined:
1. `algo.total_steps`: the total number of policy steps to compute during training (for more information, check <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/v0.5.5/howto/work_with_steps.md#policy-steps" target="_blank">here</a>).
2. `buffer.size`: the dimension of the replay buffer.
3. `algo.cnn_keys`: the keys of frames in observations that must be encoded (and eventually reconstructed by the decoder).
4. `algo.mlp_keys`: the keys of vectors in observations that must be encoded (and eventually reconstructed by the decoder).

{{% notice warning %}}
Both `algo.cnn_keys` and `algo.mlp_keys` must be non-empty lists. Moreover, the user specified keys must be a subset of the environment observation keys.
{{% /notice %}}

Below is an example of an experiment config file.
{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/configs/exp/custom_exp.yaml" >}}

{{% notice note %}}
When defining the configurations of the experiment you can specify how frequently save checkpoints of the model, and if you want to save the final agent. For more information, check <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/v0.5.5/howto/logs_and_checkpoints.md#checkpointing" target="_blank">here</a>.
{{% /notice %}}


##### Train and Evaluate the Agent
To run the experiment you just need to go into the `./sheeprl` folder and run the following command:
```shell
diambra run -s=2 python train.py exp=custom_exp
```

{{% notice note %}}
You have to instantiate 2 docker containers because sheeprl automatically performs a test of the agent after training.
{{% /notice %}}

After training, you can decide to evaluate the agent as many times as you want. You can specify only a few parameters for evaluating your agent:
1. The checkpoint of the agent that you want to evaluate (`checkpoint_path`, mandatory).
2. The type of device on which you want to run the evaluation (`fabric.device`, default to `cpu`).
3. Whether or not to capture the video of the evaluation (`env.capture_video`, default to `True`).

The reason why only these three parameters need to be specified is to avoid inconsistencies, e.g. the checkpoint of one agent and the configurations of the evaluation refer to another one, or the model in the checkpoint has different dimensions from the model specified in the configurations. 
This implies, however, that the evaluation script expects a certain directory structure. For this reason, the structure of the log directory should not be changed: all of it can be moved, but not the checkpoint individually, otherwise the script cannot automatically retrieve the environment and agent configurations.

{{< github_code "https://raw.githubusercontent.com/Eclectic-Sheep/sheeprl/v0.5.5/sheeprl/configs/eval_config.yaml" >}}

To evaluate the agent you just need to run the following command:
```shell
diambra run python evaluate.py checkpoint_path=/path/to/checkpoint.ckpt
```

If you want to specify the device to use, for instance `cuda`, you have to run the following command:
```shell
diambra run python evaluate.py checkpoint_path=/path/to/checkpoint.ckpt fabric.device=cuda
```

If you want to specify whether or not to capture the video, you have to run the following command:
```shell
diambra run python evaluate.py checkpoint_path=/path/to/checkpoint.ckpt env.capture_video=True
```


##### Train and Evaluate Scripts
In this section, we show the two scripts for training and evaluating agents. With regard to training, first the environment selected by the user is checked, if it is not one of diambra, then an exception is raised. Next, the `run()` function of SheepRL is called, which will initialize all components and start the training.

As far as evaluation is concerned, simply the configurations are passed directly to the `evaluate()` function of sheeprl. There is no need to check the environment as it has already been checked before training.

The `train.py` script:
{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/train.py" >}}

The `evaluate.py` script:
{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/evaluate.py" >}}


##### PPO Implementation
In this paragraph, we quote the code of our ppo implementation (the `ppo.py` file in the <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/v0.5.5/sheeprl/algos/ppo" target="_blank">SheepRL PPO folder</a>), just to give more context on how SheepRL works. In the `main()` function, all the components needed for training are instantiated (i.e., the agent, the environments, the buffer, the logger, and so on). Then, the environment interaction is performed, and after collecting the rollout steps, the train function is called.

The `train()` function is responsible for sharing the data between processes, if more processes are launched and the `buffer.share_data` is set to `True`. Then, for each batch, the losses are computed and the agent is updated.

{{< github_code "https://raw.githubusercontent.com/Eclectic-Sheep/sheeprl/v0.5.5/sheeprl/algos/ppo/ppo.py" >}}

#### Parallel Environments
In addition to what is seen in previous examples, this one demonstrates how to run training using parallel environments. In this example, the same PPO algorithm is used as before.
To train the agent with multiple parallel environments, you need to define properly a few environment parameters and then run the script instantiating the correct number of docker containers.

You can create a `custom_parallel_env.yaml` config file that inherits the configurations from the `custom_env.yaml` file:
{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/configs/env/custom_parallel_env.yaml" >}}

{{% notice note %}}
If you set the `env.sync_env` to `False`, then you must instantiate one more docker container because the `gymnasium.vector.AsyncVectorEnv` instantiates a dummy env when defined.
{{% /notice %}}

Then, you have to create a new file for the experiment (`custom_parallel_env_exp.yaml`), this file inherits the configurations of the `custom_exp` file and overrides the environment with the newly defined configurations (`custom_parallel_env`):
{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/configs/exp/custom_parallel_env_exp.yaml" >}}

How to run it:

```shell
# s=6 comes from: 4 for the envs, 1 for testing, 1 for `gymnasium.vector.AsyncVectorEnv`
diambra run -s=6 python train.py exp=custom_parallel_env_exp
```

### Advanced

#### Fabric
SheepRL allows training to be distributed thanks to <a href="https://lightning.ai/docs/fabric/stable/" target="_blank">Lightning Fabric</a>.

The default Fabric configuration is the following:
{{< github_code "https://raw.githubusercontent.com/Eclectic-Sheep/sheeprl/v0.5.5/sheeprl/configs/fabric/default.yaml" >}}

{{% notice note %}}
The `sheeprl.utils.callback.CheckpointCallback` is used for saving the checkpoint during training and for saving the trained agent.
{{% /notice %}}

To modify the Fabric configs, you can add a `fabric` field in the experiment file, as shown below. In this case, we selected `2` devices, the accelerator is `"cuda"` and the training is performed in 16 bits. As before, it inherits the configurations from the `custom_exp` and then sets the Fabric parameters.
{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/configs/exp/custom_fabric_exp.yaml" >}}

How to run it:
```shell
# Remember to set properly the number of containers to create
#   - Each process has 1 environment
#   - There are 2 processes
#   - Only the zero-rank process will perform the evaluation after the training
diambra run -s=3 python train.py exp=custom_fabric_exp
```

{{% notice warning %}}
To run the fabric experiment, make sure you have a `cuda` GPU in your device, otherwise, change the device from `cuda` to `cpu` (or to another device).
{{% /notice %}}


#### Metric and Logging
Finally, SheepRL allows you to visualize and monitor training using Tensorboard. 

{{% notice note %}}
We strongly recommend to read the SheepRL <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/v0.5.5/howto/logs_and_checkpoints.md" target="_blank">logging documentation</a> to know about how to enable/disable logging.
{{% /notice %}}

Below is reported the default logging configuration and a table describing the arguments.

{{< github_code "https://raw.githubusercontent.com/Eclectic-Sheep/sheeprl/v0.5.5/sheeprl/configs/metric/default.yaml" >}}

| <strong><span style="color:#5B5B60;">Argument</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                       |
| ------------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `log_every`  | `int` | `5000` | Number of steps between one log and the next |
| `disable_timer` | `bool` | `False` | Whether or not to disable timer information (training and environment interaction) |
| `log_level` | `int` | `1` | The level of logging (`0`: disabled, `1`: log everything) |
| `sync_on_compute` | `bool` | `False` | Whether to synchronize the metrics between processes |
| `aggregator` | `Dict[str, Any]` | - | Configurations of the aggregator to be instantiated, containing the metrics to log |


You can modify the default metric configurations by adding in the `custom_exp` file the custom configuration you want under the `metric` key, as shown below.
In this example, we do not log the timer information and we want to synchronize the metrics between the 2 processes. Moreover, we add 3 metrics to log to the aggregator (in addition to reward and episode length): the value loss, the policy loss, and the entropy loss.
{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/configs/exp/custom_metric_exp.yaml" >}}

How to run it:

```shell
# s=3 since `custom_metric_exp` extends from the fabric experiments
diambra run -s=3 python train.py exp=custom_metric_exp
```

The logs are stored in the `./logs/runs/<algo_name>/<env_id>/<datetime_experiment>/` folder, and to visualize the plots, you just need to run the following command:

```bash
tensorboard --logdir /path/to/logging/directory
```
open your browser and go to `http://localhost:6006/`. You can eventually modify the port of the process, for instance, you can use port `6010` by running the following command:

```bash
tensorboard --logdir /path/to/logging/directory --port 6010
```

#### Agent Script for Competition
Finally, after the agent training is completed, besides running it locally on your own machine, you may want to submit it to our <a href="../../competitionplatform">Competition Platform!</a> To do so, you can use the following script that provides a ready-to-use, flexible example that can accommodate different games and settings.

{{% notice tip %}}
To submit your trained agent to our platform, compete for the first leaderboard positions, and unlock our achievements, follow the simple steps described in the <a href="../../competitionplatform/howtosubmitanagent/">"How to Submit an Agent"</a> section.
{{% /notice %}}

The script for submitting PPOs is shown below.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/agent-ppo.py" >}}

If you have trained an agent different from PPO, you can simply reuse this script, modifying a couple of things:
1. The `build_agent()` method: every agent has its own method, you can find it in the `./sheeprl/algos/<algo_name>/agent.py` file. You should check the parameters of the method, every agent has its own models, so you must pass the state of the agent's models as a parameter. The parameter is always called: `model_name_state`, you can retrieve the state of the model from the `state` dictionary (in the script shown above) with the `"model_name"` key.
2. Whether or not to initialize the recurrent states: the models with recurrent neural networks (e.g., all the Dreamer algorithms) need to initialize the recurrent states at every reset of the environment. It is recommended to look at the test function of the algorithm you want to submit.

{{% notice tip %}}
The only algorithm you need to pay a little more attention to is PPO Recurrent. It is recommended to look at its test function available <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/v0.5.5/sheeprl/algos/ppo_recurrent/utils.py#L25" target="_blank">here</a>.
{{% /notice %}}

For example, the changes to be made to the script to submit DreamerV3 are explained below.

```diff
import argparse
import json

import gymnasium as gym
import torch
from lightning import Fabric
from omegaconf import OmegaConf
-from sheeprl.algos.ppo.agent import build_agent
+from sheeprl.algos.dreamer_v3.agent import build_agent
-from sheeprl.algos.ppo.utils import prepare_obs
+from sheeprl.algos.dreamer_v3.utils import prepare_obs
from sheeprl.utils.env import make_env
from sheeprl.utils.utils import dotdict

def main(cfg_path: str, checkpoint_path: str, test=False):
    ...

    agent = build_agent(
        fabric=fabric,
        actions_dim=actions_dim,
        is_continuous=False,
        cfg=cfg,
        obs_space=observation_space,
-       agent_state=state["agent"],
+       world_model_state=state["world_model"],
+       actor_state=state["actor"],
+       critic_state=state["critic"],
+       target_critic_state=state["target_critic"],
    )[-1]
    agent.eval()

    # Print policy network architecture
    print("Policy architecture:")
    print(agent)

    obs, info = env.reset()
+   agent.init_states()

    while True:
        ...

        if terminated or truncated:
            obs, info = env.reset()
+           agent.init_states()
            if info["env_done"] or test is True:
                break

    ...

```

The final script for the submission of dreamer_v3 is shown below.

{{< github_code "https://raw.githubusercontent.com/diambra/agents/main/sheeprl/agent-dreamer_v3.py" >}}

