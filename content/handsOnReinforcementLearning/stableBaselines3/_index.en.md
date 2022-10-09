---
title: Stable Baselines 3
weight: 10
---

<div style="font-size:1.125rem;">

### Index

- <a href="./#getting-ready">Getting Ready</a>
- <a href="./#basic">Basic</a>
- <a href="./#advanced">Advanced</a>

</div>

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

This should be enough to prepare your system to execute the following examples. You can refer to the official <a href="https://stable-baselines3.readthedocs.io/en/master/guide/install.html" target="_blank">Stable Baselines 3 documentation</a> or reach out on our <a href="https://discord.gg/tFDS2UN5sv" target="_blank">Discord server</a> for specific needs.

All the examples presented below are available here: <a href="https://github.com/diambra/agents/tree/main/stable_baselines3" target="_blank">DIAMBRA Agents - Stable Baselines 3</a>. They have been created following the high level approach found on <a href="https://stable-baselines3.readthedocs.io/en/master/guide/examples.html" target="_blank">Stable Baselines 3 examples</a> page, thus allowing to easily extend them and to understand how they interface with the different components.

These examples only aims at demonstrating the core functionalities and high level aspects, they will not generate well performing agents, even if the training time is extended to cover a large number of training steps. The user will need to build upon them, exploring aspects like: policy network architecture, algorithm hyperparameter tuning, observation space tweaking, rewards wrapping and other similar ones.

#### Native interface

DIAMBRA Arena native interface with Stable Baselines 3 covers a wide range of use cases, automating handling of vectorized environments and monitoring wrappers. In the majority of cases it will be sufficient for users to directly import and use it, with no need for additional customization. Below is reported its interface and a table describing its arguments.

```python
def make_sb3_env(game_id, env_settings={}, wrappers_settings=None,
                 use_subprocess=True, seed=0, log_dir_base="/tmp/DIAMBRALog/",
                 start_index=0, allow_early_resets=True,
                 start_method=None, no_vec=False):
```

| <strong><span style="color:#5B5B60;">Argument</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Default Value(s)</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>                                                                                                        |
| ------------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `game_id`                                                     | `string`                                                  | -                                                                     | Game environment identifier.                                                                                                                                            |
| `env_settings`                                                | `dict`                                                    | `{}`                                                                  | Environment settings (<a href="/envs/#settings">see more</a>).                                                                                                          |
| `wrappers_settings`                                           | `dict`                                                    | `None`                                                                | Wrappers settings (<a href="/wrappers/">see more</a>).                                                                                                                  |
| `use_subprocess`                                              | `bool`                                                    | `True`                                                                | If to use subprocesses for multi-threaded parallelization.                                                                                                              |
| `seed`                                                        | `int`                                                     | 0                                                                     | Random number generator seed.                                                                                                                                           |
| `log_dir_base`                                                | `string`                                                  | `"/tmp/DIAMBRALog/"`                                                  | Folder where to save execution logs.                                                                                                                                    |
| `start_index`                                                 | `int`                                                     | 0                                                                     | Starting process rank index.                                                                                                                                            |
| `allow_early_resets`                                          | `bool`                                                    | `True`                                                                | Monitor wrapper argument to allow environment reset before it is done                                                                                                   |
| `start_method`                                                | `string`                                                  | `None`                                                                | Method to spawn subprocesses when active (<a href="https://stable-baselines3.readthedocs.io/en/master/guide/vec_envs.html#subprocvecenv" target="_blank">see more</a>). |
| `no_vec`                                                      | `bool`                                                    | `False`                                                               | If `True` avoids using vectorized environments (valid only when using a single instance). reset.                                                                        |

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

```python
import diambra.arena
from stable_baselines3 import A2C

if __name__ == "__main__":

    env = diambra.arena.make("doapp", {"hardcore": True,
                                       "frame_shape": [128, 128, 1]})

    print("\nStarting training ...\n")
    agent = A2C('CnnPolicy', env, verbose=1)
    agent.learn(total_timesteps=200)
    print("\n .. training completed.")

    print("\nStarting trained agent execution ...\n")
    observation = env.reset()
    while True:
        env.render()

        action, _state = agent.predict(observation, deterministic=True)

        observation, reward, done, info = env.step(action)

        if done:
            observation = env.reset()
            break
    print("\n... trained agent execution completed.\n")

    env.close()
```

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

```python
import diambra.arena
from stable_baselines3 import A2C
from stable_baselines3.common.evaluation import evaluate_policy

if __name__ == "__main__":

    # Create environment
    env = diambra.arena.make("doapp", {"hardcore": True,
                                       "frame_shape": [128, 128, 1]})

    # Instantiate the agent
    agent = A2C('CnnPolicy', env, verbose=1)
    # Train the agent
    agent.learn(total_timesteps=200)
    # Save the agent
    agent.save("a2c_doapp")
    # Delete trained agent to demonstrate loading
    del agent

    # Load the trained agent
    # NOTE: if you have loading issue, you can pass `print_system_info=True`
    # to compare the system on which the agent was trained vs the current one
    # agent = A2C.load("a2c_doapp", env=env, print_system_info=True)
    agent = A2C.load("a2c_doapp", env=env)

    # Evaluate the agent
    # NOTE: If you use wrappers with your environment that modify rewards,
    #       this will be reflected here. To evaluate with original rewards,
    #       wrap environment in a "Monitor" wrapper before other wrappers.
    mean_reward, std_reward = evaluate_policy(agent, agent.get_env(), n_eval_episodes=3)
    print("Reward: {} (avg) ± {} (std)".format(mean_reward, std_reward))

    # Run trained agent
    observation = env.reset()
    cumulative_reward = 0
    while True:
        env.render()

        action, _state = agent.predict(observation, deterministic=True)

        observation, reward, done, info = env.step(action)
        cumulative_reward += reward
        if (reward != 0):
            print("Cumulative reward =", cumulative_reward)

        if done:
            observation = env.reset()
            break

    env.close()
```

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

```python
import diambra.arena
from diambra.arena.stable_baselines3.make_sb3_env import make_sb3_env
from stable_baselines3 import PPO

if __name__ == '__main__':

    # Settings
    settings = {}
    settings["hardcore"] = True
    settings["frame_shape"] = [128, 128, 1]
    settings["characters"] = [["Kasumi"], ["Kasumi"]]

    # Wrappers Settings
    wrappers_settings = {}
    wrappers_settings["reward_normalization"] = True
    wrappers_settings["frame_stack"] = 5

    # Create environment
    env, num_envs = make_sb3_env("doapp", settings, wrappers_settings)
    print("Activated {} environment(s)".format(num_envs))

    print("Observation space shape =", env.observation_space.shape)
    print("Observation space type =", env.observation_space.dtype)

    print("Act_space =", env.action_space)

    # Instantiate the agent
    agent = PPO('CnnPolicy', env, verbose=1)

    # Print policy network architecture
    print("Policy architecture:")
    print(agent.policy)

    # Train the agent
    agent.learn(total_timesteps=200)

    # Run trained agent
    observation = env.reset()
    cumulative_reward = [0.0 for _ in range(num_envs)]
    while True:
        env.render()

        action, _state = agent.predict(observation, deterministic=True)

        observation, reward, done, info = env.step(action)
        cumulative_reward += reward
        if any(x != 0 for x in reward):
            print("Cumulative reward(s) =", cumulative_reward)

        if done.any():
            observation = env.reset()
            break

    env.close()
```

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

```python
import diambra.arena
from diambra.arena.stable_baselines3.make_sb3_env import make_sb3_env
from stable_baselines3 import PPO

if __name__ == "__main__":

    # Settings
    settings = {}
    settings["frame_shape"] = [128, 128, 1]
    settings["characters"] = [["Kasumi"], ["Kasumi"]]

    # Wrappers Settings
    wrappers_settings = {}
    wrappers_settings["reward_normalization"] = True
    wrappers_settings["actions_stack"] = 12
    wrappers_settings["frame_stack"] = 5
    wrappers_settings["scale"] = True
    wrappers_settings["exclude_image_scaling"] = True
    wrappers_settings["flatten"] = True
    wrappers_settings["filter_keys"] = ["stage", "P1_ownHealth", "P1_oppHealth",
                                        "P1_ownSide", "P1_oppSide", "P1_oppChar",
                                        "P1_actions_move", "P1_actions_attack"]

    # Create environment
    env, num_envs = make_sb3_env("doapp", settings, wrappers_settings)
    print("Activated {} environment(s)".format(num_envs))

    print("Observation space =", env.observation_space)
    print("Act_space =", env.action_space)

    # Instantiate the agent
    agent = PPO("MultiInputPolicy", env, verbose=1)

    # Print policy network architecture
    print("Policy architecture:")
    print(agent.policy)

    # Train the agent
    agent.learn(total_timesteps=200)

    # Run trained agent
    observation = env.reset()
    cumulative_reward = [0.0 for _ in range(num_envs)]
    while True:
        env.render()

        action, _state = agent.predict(observation, deterministic=True)

        observation, reward, done, info = env.step(action)
        cumulative_reward += reward
        if any(x != 0 for x in reward):
            print("Cumulative reward(s) =", cumulative_reward)

        if done.any():
            observation = env.reset()
            break

    env.close()
```

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

```python
import os
import time
import yaml
import json
import argparse
from diambra.arena.stable_baselines3.make_sb3_env import make_sb3_env
from diambra.arena.stable_baselines3.sb3_utils import linear_schedule, AutoSave
from stable_baselines3 import PPO

if __name__ == "__main__":

    parser = argparse.ArgumentParser()
    parser.add_argument("--cfgFile", type=str, required=True,
                        help="Configuration file")
    opt = parser.parse_args()
    print(opt)

    # Read the cfg file
    yaml_file = open(opt.cfgFile)
    params = yaml.load(yaml_file, Loader=yaml.FullLoader)
    print("Config parameters = ", json.dumps(params, sort_keys=True, indent=4))
    yaml_file.close()

    time_dep_seed = int((time.time() - int(time.time() - 0.5)) * 1000)
    base_path = os.path.dirname(os.path.abspath(__file__))
    model_folder = os.path.join(base_path, params["folders"]["parent_dir"],
                                params["settings"]["game_id"],
                                params["folders"]["model_name"], "model")
    tensor_board_folder = os.path.join(base_path, params["folders"]["parent_dir"],
                                       params["settings"]["game_id"],
                                       params["folders"]["model_name"], "tb")

    os.makedirs(model_folder, exist_ok=True)

    # Settings
    settings = params["settings"]

    # Wrappers Settings
    wrappers_settings = params["wrappers_settings"]

    # Create environment
    env, num_envs = make_sb3_env(params["settings"]["game_id"],
                                 settings, wrappers_settings, seed=time_dep_seed)
    print("Activated {} environment(s)".format(num_envs))

    print("Observation space =", env.observation_space)
    print("Act_space =", env.action_space)

    # Policy param
    policy_kwargs = params["policy_kwargs"]

    # PPO settings
    ppo_settings = params["ppo_settings"]
    gamma = ppo_settings["gamma"]
    model_checkpoint = ppo_settings["model_checkpoint"]

    learning_rate = linear_schedule(ppo_settings["learning_rate"][0],
                                    ppo_settings["learning_rate"][1])
    clip_range = linear_schedule(ppo_settings["clip_range"][0],
                                 ppo_settings["clip_range"][1])
    clip_range_vf = clip_range
    batch_size = ppo_settings["batch_size"]
    n_epochs = ppo_settings["n_epochs"]
    n_steps = ppo_settings["n_steps"]

    if model_checkpoint == "0M":
        # Initialize the agent
        agent = PPO("MultiInputPolicy", env, verbose=1,
                    gamma=gamma, batch_size=batch_size,
                    n_epochs=n_epochs, n_steps=n_steps,
                    learning_rate=learning_rate, clip_range=clip_range,
                    clip_range_vf=clip_range_vf, policy_kwargs=policy_kwargs,
                    tensorboard_log=tensor_board_folder)
    else:
        # Load the trained agent
        agent = PPO.load(os.path.join(model_folder, model_checkpoint), env=env,
                         gamma=gamma, learning_rate=learning_rate,
                         clip_range=clip_range, clip_range_vf=clip_range_vf,
                         policy_kwargs=policy_kwargs,
                         tensorboard_log=tensor_board_folder)


    # Print policy network architecture
    print("Policy architecture:")
    print(agent.policy)

    # Create the callback: autosave every USER DEF steps
    autosave_freq = ppo_settings["autosave_freq"]
    auto_save_callback = AutoSave(check_freq=autosave_freq, num_envs=num_envs,
                                  save_path=os.path.join(model_folder,
                                                         model_checkpoint + "_"))

    # Train the agent
    time_steps = ppo_settings["time_steps"]
    agent.learn(total_timesteps=time_steps, callback=auto_save_callback)

    # Save the agent
    new_model_checkpoint = str(int(model_checkpoint[:-1]) + time_steps) + "M"
    model_path = os.path.join(model_folder, new_model_checkpoint)
    agent.save(model_path)

    # Close the environment
    env.close()
```

How to run it:

```shell
diambra run python training.py --cfgFile path/to/config.yaml
```

and the configuration file to be used with this training script is reported below:

```yaml
folders:
  parent_dir: "./results/"
  model_name: "sr6_128x4_das_nc"

settings:
  game_id: "doapp"
  characters: [["Kasumi"], ["Kasumi"]]
  difficulty: 3
  step_ratio: 6
  frame_shape: [128, 128, 1]
  continue_game: 0.0
  action_space: "discrete"
  attack_but_combination: false
  char_outfits: [2, 2]
  player: "Random"
  show_final: false

wrappers_settings:
  frame_stack: 4
  dilation: 1
  actions_stack: 12
  reward_normalization: true
  scale: true
  exclude_image_scaling: true
  flatten: true
  filter_keys:
    [
      "stage",
      "P1_ownHealth",
      "P1_oppHealth",
      "P1_ownSide",
      "P1_oppSide",
      "P1_oppChar",
      "P1_actions_move",
      "P1_actions_attack",
    ]

policy_kwargs:
  #net_arch: [{ pi: [64, 64], vf: [32, 32] }]
  net_arch: [64, 64]

ppo_settings:
  gamma: 0.94
  model_checkpoint: "0M"
  learning_rate: [2.5e-4, 2.5e-6] # To start
  clip_range: [0.15, 0.025] # To start
  #learning_rate: [5.0e-5, 2.5e-6] # Fine Tuning
  #clip_range: [0.075, 0.025] # Fine Tuning
  batch_size:
    256 # 8 #nminibatches gave different batch size depending on
    # the number of environments: batch_size = (n_steps * n_envs) // nminibatches
  n_epochs: 4
  n_steps: 128
  autosave_freq: 256
  time_steps: 512
```
