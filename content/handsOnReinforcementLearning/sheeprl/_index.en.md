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
  - <a href="#parallel-environments">Parallel Environments</a>
- <a href="#advanced">Advanced</a>
  - <a href="#fabric">Fabric</a>
  - <a href="#metric-and-logging">Metric and Logging</a>

</div>

{{% notice tip %}}
The source code of all examples described in this section is available in our <a href="#" target="_blank">DIAMBRA Agents</a> repository.
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

This should be enough to prepare your system to execute the following examples. You can refer to the official <a href="https://github.com/Eclectic-Sheep/sheeprl" target="_blank">SheepRL documentation</a> or reach out on our <a href="https://diambra.ai/discord" target="_blank">Discord server</a> for specific needs.

All the examples presented below are available here: <a href="#" target="_blank">DIAMBRA Agents - SheepRL</a>. They have been created following the high level approach found on <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/main/howto/learn_in_diambra.md" target="_blank">SheepRL DIAMBRA</a> page, thus allowing to easily extend them and to understand how they interface with the different components.

These examples only aim at demonstrating the core functionalities and high-level aspects, they will not generate well-performing agents, even if the training time is extended to cover a large number of training steps. The user will need to build upon them, exploring aspects like policy network architecture, algorithm hyperparameter tuning, observation space tweaking, rewards wrapping, and other similar ones.

#### General Environment Settings
SheepRL provides a lot of different environments that share a set of parameters. Moreover, SheepRL leverages <a href="https://hydra.cc/" target="_blank">Hydra</a> for defining hierarchical configurations. Below is reported the general structure of the configuration of an environment and a table describing the arguments.

```yaml
id: ???
num_envs: 4
frame_stack: 1
sync_env: False
screen_size: 64
action_repeat: 1
grayscale: False
clip_rewards: False
capture_video: True
frame_stack_dilation: 1
max_episode_steps: null
reward_as_observation: False
wrapper:
  ...
```
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
If you have never used Hydra, before continuing, it is strongly recommended to check the <a href="https://hydra.cc/" target="_blank">Hydra official documentation</a> and the <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/main/howto/configs.md" target="_blank">SheepRL-related section</a>.
{{% /notice %}}


#### Native interface

DIAMBRA Arena native interface with SheepRL covers a wide range of use cases, automating the handling of vectorized environments and monitoring wrappers. In the majority of cases, it will be sufficient for users to directly import and use it, with no need for additional customization. Below is reported its interface and a table describing its arguments.

```python
class DiambraWrapper(gym.Wrapper):
    def __init__(
        self,
        id: str,
        action_space: str = "diambra.arena.SpaceTypes.DISCRETE",
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
| `action_space` | `str` | `"diambra.arena.SpaceTypes.DISCRETE"` | Which action space to use: one between `"diambra.arena.SpaceTypes.DISCRETE"` and `"diambra.arena.SpaceTypes.MULTI_DISCRETE"` |
| `screen_size` | `int \| Tuple[int, int]` | `64` | Screen size of the frames |
| `grayscale` | `bool` | `False` | Whether to use grayscale frames |
| `rank` | `int` | `0` | Rank of the environment |
| `diambra_settings` | `Dict[str, Any]` | `{}` | The settings of the environment. See <a href="../../envs/#settings" target="_blank">here</a> to check which settings you can specify. |
| `diambra_wrappers` | `Dict[str, Any]` | `{}` | The wrappers to apply to the environment. See <a href="../../wrappers/" target="_blank">here</a> to check which wrappers you can specify. |
| `render_mode` | `str` | `"rgb_array"` | Rendering mode |
| `log_level` | `int` | `0` | Log level |
| `increase_performance` | `bool` | `True` | Whether to modify frames on the engine side (`True`) or use the wrapper (`False`) |

{{% notice note %}}
For the interface low-level details, users can review the correspondent source code <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/main/sheeprl/envs/diambra.py" target="_blank">here</a>.
{{% /notice %}}


#### Agent Settings
SheepRL provides several SOTA algorithms, both model-free and model-based. <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/main/sheeprl/configs/algo" target="_blank">Here</a> you can find the default configurations for these agent. Of course, one can change algorithm-related hyper-parameters for customizing his/her experiments.

### Basic
As anticipated before, SheepRL provides several default configurations for all its components, these configurations are available and can be composed to set up an experiment. Otherwise, it is possible to custom define the configurations of all or some components: the two main components to define for the experiments are the agent and the environment. 

Regarding the environment, there are some constraints that must be respected, for example, the dictionary observation spaces cannot be nested. For this reason, the DIAMBRA <a href="../../wrappers/#flatten-and-filter-observation" target="_blank">flattening wrapper</a> is always used. For more information about the constraints of the SheepRL library, check <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/main/howto/learn_in_diambra.md#args" target="_blank">here</a>.

Instead, regarding the agent, the only constraints that are present concern the action space that agents support. You can read the supported action spaces in Table 2 of the <a href="https://github.com/Eclectic-Sheep/sheeprl" target="_blank">README</a> in the SheepRL GitHub repository.


#### Customising the Configurations
The default configurations are available <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/main/sheeprl/configs" target="_blank">here</a>. If you want to define your custom experiments, you just need to follow a few steps:
1. You need to create a folder (with the same structure as the <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/main/sheeprl/configs" target="_blank">SheepRL configs folder</a>) where to place your custom configurations. 
2. You need to define the `SHEEPRL_SEARCH_PATH` environment variable in the `.env` file as follows: `SHEEPRL_SEARCH_PATH=file://path/to/custom/configs/folder;pkg://sheeprl.configs`.
3. You need to define the custom configurations, being careful that the filename is different from the default ones. If this is not respected, your file will overwrite the default configurations.

#### Basic Example

This example demonstrates how to:

* Leverage SheepRL to define the environment for training.
* Define a PPO Agent to be trained.
* Define custom configurations for your experiment.
* Train the agent.
* Run the trained agent in the environment for one episode.


SheepRL natively supports supports dictionary observation spaces, the only thing you need to define is the keys of the observations you want to process. For more information about observations selection, check <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/main/howto/select_observations.md" target="_blank">here</a>.

##### Configs Folder
First, it is necessary to create a folder for the configuration files. We create the `configs` folder under the `./agents/sheeprl/` folder in the <a href="https://github.com/diambra/agents/tree/main/stable_baselines3" target="_blank">DIAMBRA Arena</a> GitHub repository. Then we added the `.env` file in `./agents/sheeprl` folder, in which we need to define the `SHEEPRL_SEARCH_PATH` environment variable as follows:
```bash
SHEEPRL_SEARCH_PATH=file://agents/sheeprl/configs;pkg://sheeprl.configs
```

##### Define the Environment
Now, in the `./agents/sheeprl/configs` folder we create the `env` folder in which the `custom_env.yaml` will be placed.
Below is reported a possible configuration of the environment.

```yaml
defaults:
  - default
  - _self_

# Override from `default` config
# `default` config contains the arguments shared
# among all the environments in SheepRL
id: doapp
frame_stack: 1
sync_env: True
action_repeat: 1
num_envs: 1
screen_size: 64
grayscale: False
clip_rewards: False
capture_video: True
frame_stack_dilation: 1
max_episode_steps: null
reward_as_observation: False

# DOAPP-related arguments
wrapper:
  # class to be instantiated
  _target_: sheeprl.envs.diambra.DiambraWrapper
  id: ${env.id}
  action_space: diambra.arena.SpaceTypes.DISCRETE 
    # or `diambra.arena.SpaceTypes.MULTI_DISCRETE`
  screen_size: ${env.screen_size}
  grayscale: ${env.grayscale}
  repeat_action: ${env.action_repeat}
  rank: null
  log_level: 0
  increase_performance: True
  diambra_settings:
    role: diambra.arena.Roles.P1 # or `diambra.arena.Roles.P1` or `null`
    step_ratio: 6
    difficulty: 4
    continue_game: 0.0
    show_final: False
    outfits: 2
    splash_screen: False
  diambra_wrappers:
    stack_actions: 1
    no_op_max: 0
    no_attack_buttons_combinations: False
    add_last_action: True
    scale: False
    exclude_image_scaling: False
    process_discrete_binary: False
    role_relative: True
```

##### Define the Agent
As for the environment, we need to create a dedicated folder to place the custom configurations of the agents: we create the `algo` folder in the `./agents/sheeprl/configs` folder and we place the `custom_ppo_agent.yaml` file. Under the `default` keyword, it is possible to retrieve the configurations specified in another file, in our case, since we are defining the agent, we can take the configuration from the <a href="https://github.com/Eclectic-Sheep/sheeprl/tree/main/sheeprl/configs/algo" target="_blank">algorithm config folder</a> in SheepRL, in which several SOTA agents are defined.

{{% notice note %}}
When defining an agent it is mandatory to define the `name` of the algorithm. The value of these parameters defines which algorithm will be used for training. If you inherit the default configurations of a specific algorithm, then you do not need to define it, since it is already defined in the default configs of that algorithm.
{{% /notice %}}

Below is reported a configuration file for a PPO agent.
```yaml
defaults:
  # Take default configurations of PPO
  - ppo
  # define Adam optimizer under the `optimizer` key 
  # from the sheeprl/configs/optim folder
  - override /optim@optimizer: adam
  - _self_

# Override default ppo arguments
# `name` is a mandatory attribute, it must be equal to the algorithm name
# If you inherit the default configurations of a specific algoritm,
# then you do not need to define it, since it is already defined in the default configs
name: ppo
update_epochs: 1
normalize_advantages: True
rollout_steps: 32
dense_units: 16
mlp_layers: 1
dense_act: torch.nn.Tanh
max_grad_norm: 1.0

# Encoder
encoder:
  cnn_features_dim: 128
  mlp_features_dim: 32
  dense_units: ${algo.dense_units}
  mlp_layers: ${algo.mlp_layers}
  dense_act: ${algo.dense_act}
  layer_norm: ${algo.layer_norm}

# Actor
actor:
  dense_units: ${algo.dense_units}
  mlp_layers: ${algo.mlp_layers}
  dense_act: ${algo.dense_act}
  layer_norm: ${algo.layer_norm}

# Critic
critic:
  dense_units: ${algo.dense_units}
  mlp_layers: ${algo.mlp_layers}
  dense_act: ${algo.dense_act}
  layer_norm: ${algo.layer_norm}

# Single optimizer for both actor and critic
optimizer:
  lr: 5e-3
  eps: 1e-6

```

##### Define the Experiment
The last thing to do is to define the experiment. You just need to define a `custom_exp.yaml` file in the `./agents/sheeprl/configs/exp` folder and assemble the environment, the agent, and the other components of the SheepRL framework. In particular, there are four parameters that must be defined:
1. `total_steps`: the total number of policy steps to compute during training (for more information, check <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/main/howto/work_with_steps.md#policy-steps" target="_blank">here</a>).
2. `buffer.size`: the dimension of the replay buffer.
3. `cnn_keys`: the keys of frames in observations that must be encoded (and eventually reconstructed by the decoder).
4. `mlp_keys`: the keys of vectors in observations that must be encoded (and eventually reconstructed by the decoder).

Below is an example of an experiment config file.
```yaml
# @package _global_

defaults:
  # Selects the algorithm and the environment
  - override /algo: custom_ppo_agent
  - override /env: custom_env
  - _self_

# Experiment
total_steps: 1024
per_rank_batch_size: 16

# Buffer
buffer:
  share_data: False
  size: ${algo.rollout_steps}

checkpoint:
  save_last: True

cnn_keys:
  encoder: [frame]
mlp_keys:
  encoder:
    - own_character
    - own_health
    - own_side
    - own_wins
    - opp_character
    - opp_health
    - opp_side
    - opp_wins
    - stage
    - timer
    - action
```

{{% notice note %}}
When defining the configurations of the experiment you can specify how frequently save checkpoints of the model, and if you want to save the final agent. For more information, check <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/main/howto/logs_and_checkpoints.md#checkpointing" target="_blank">here</a>.
{{% /notice %}}


##### Train and Evaluate the Agent

To run the experiment you just need to go into the `./sheeprl` folder and run the following command:
```shell
diambra run -s=2 python train.py exp=custom_exp
```

{{% notice note %}}
You have to instantiate 2 docker containers because sheeprl automatically performs a test of the agent after training.
{{% /notice %}}

After training, you can decide to evaluate the agent as many times as you want. The only thing you need is the checkpoint of the agent that you want to evaluate.
To evaluate the agent you just need to run the following command:
```shell
diambra run python evaluate.py checkpoint_path=/path/to/checkpoint.ckpt
```


#### Parallel Environments

In addition to what is seen in previous examples, this one demonstrates how to run training using parallel environments. In this example, the same PPO algorithm is used as before.
To train the agent with multiple parallel environments, you need to define properly a few environment parameters and then run the script instantiating the correct number of docker containers.

The `custom_env` config file must be modified as follows:
```yaml
...
sync_env: False # True if you want to use the gymnasium.vector.SyncVectorEnv
num_envs: 4
...
```

{{% notice note %}}
If you set the `env.sync_env` to `False`, then you must instantiate one more docker container because the `gymnasium.vector.AsyncVectorEnv` instantiates a dummy env when defined.
{{% /notice %}}

How to run it:

```shell
# s=6 comes from: 4 for the envs, 1 for testing, 1 for `gymnasium.vector.AsyncVectorEnv`
diambra run -s=6 python train.py exp=custom_exp
```

### Advanced

#### Fabric
SheepRL allows training to be distributed thanks to <a href="https://lightning.ai/docs/fabric/stable/" target="_blank">Lightning Fabric</a>.

The default Fabric configuration is the following:
{{< github_code "https://raw.githubusercontent.com/Eclectic-Sheep/sheeprl/main/sheeprl/configs/fabric/default.yaml" >}}

{{% notice note %}}
The `sheeprl.utils.callback.CheckpointCallback` is used for saving the checkpoint during training and for saving the trained agent.
{{% /notice %}}

To modify the Fabric configs, you can add a `fabric` field in the experiment file, as shown below. In this case, we selected `2` devices, the accelerator is `"cuda"` and the training is performed in 16 bits.
```yaml
# @package _global_

defaults:
  # Selects the algorithm and the environment
  - override /algo: custom_ppo_agent
  - override /env: custom_env
  - _self_

# Experiment
total_steps: 1024
per_rank_batch_size: 16

# Buffer
buffer:
  share_data: False
  size: ${algo.rollout_steps}

checkpoint:
  save_last: True

cnn_keys:
  encoder: [frame]
mlp_keys:
  encoder:
    - own_character
    - own_health
    - own_side
    - own_wins
    - opp_character
    - opp_health
    - opp_side
    - opp_wins
    - stage
    - timer
    - action

fabric:
  devices: 2
  accelerator: cuda
  precision: bf16
```

#### Metric and Logging
Finally, SheepRL allows you to visualize and monitor training using Tensorboard. 

{{% notice note %}}
We strongly recommend to read the SheepRL <a href="https://github.com/Eclectic-Sheep/sheeprl/blob/main/howto/logs_and_checkpoints.md" target="_blank">logging documentation</a> to know about how to enable/disable logging.
{{% /notice %}}

Below is reported the default logging configuration and a table describing the arguments.

{{< github_code "https://raw.githubusercontent.com/Eclectic-Sheep/sheeprl/main/sheeprl/configs/metric/default.yaml" >}}

| <strong><span style="color:#5B5B60;">Argument</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                       |
| ------------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `log_every`  | `int` | `5000` | Number of steps between one log and the next |
| `disable_timer` | `bool` | `False` | Whether or not to disable timer information (training and environment interaction) |
| `log_level` | `int` | `1` | The level of logging (`0`: disabled, `1`: log everything) |
| `sync_on_compute` | `bool` | `False` | Whether to synchronize the metrics between processes |
| `aggregator` | `Dict[str, Any]` | - | Configurations of the aggregator to be instantiated, containing the metrics to log |


You can modify the default metric configurations by adding in the `custom_exp` file the custom configuration you want under the `metric` key, as shown below.
In this example, we do not log the timer information and we want to synchronize the metrics between the 2 processes. Moreover, we add 3 metrics to log to the aggregator (in addition to reward and episode length): the value loss, the policy loss, and the entropy loss.
```yaml
# @package _global_

defaults:
  # Select the algorithm and the environment
  - override /algo: custom_ppo_agent
  - override /env: custom_env
  - _self_

# Experiment
total_steps: 1024
per_rank_batch_size: 16

# Buffer
buffer:
  share_data: False
  size: ${algo.rollout_steps}

checkpoint:
  save_last: True

cnn_keys:
  encoder: [frame]
mlp_keys:
  encoder:
    - own_character
    - own_health
    - own_side
    - own_wins
    - opp_character
    - opp_health
    - opp_side
    - opp_wins
    - stage
    - timer
    - action

fabric:
  devices: 2
  accelerator: cuda
  precision: bf16

metric:
  disable_timer: True
  sync_on_compute: True
  aggregator:
    metrics:
      Loss/value_loss:
        _target_: torchmetrics.MeanMetric
        sync_on_compute: ${metric.sync_on_compute}
      Loss/policy_loss:
        _target_: torchmetrics.MeanMetric
        sync_on_compute: ${metric.sync_on_compute}
      Loss/entropy_loss:
        _target_: torchmetrics.MeanMetric
        sync_on_compute: ${metric.sync_on_compute}
```

The logs are stored in the `./logs/runs/<algo_name>/<env_id>/<datetime_experiment>/` folder, and to visualize the plots, you just need to run the following command:

```bash
tensorboard --logdir /path/to/logging/directory
```
open your browser and go to `http://localhost:6006/`. You can eventually modify the port of the process, for instance, you can use port `6010` running the following command:

```bash
tensorboard --logdir /path/to/logging/directory --port 6010
```