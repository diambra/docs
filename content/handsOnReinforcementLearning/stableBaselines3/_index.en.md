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

We highly recommend using virtual environments to isolate your python installs, especially to avoid conflicts in dependencies. In what follows we use Conda but any other tools should work too.

Create and activate a new dedicated virtual environment:

```shell
conda create -n diambra-arena-sb3 python=3.8
conda activate diambra-arena-sb3
```

Install DIAMBRA Arena with Stable Baselines 3 interface:

```shell
pip install diambra-arena[stable-baselines3]
```

This should be enough to prepare your system to execute the following examples. You can refer to the official <a href="https://stable-baselines3.readthedocs.io/en/master/guide/install.html" target="_blank">Stable Baselines 3 documentation</a> for specific needs.

All the examples presented below are available here: <a href="https://github.com/diambra/agents/tree/main/stable_baselines3" target="_blank">DIAMBRA Agents - Stable Baselines 3</a>.

### Basic

#### Basic Example

```python
import diambra.arena
from stable_baselines3 import A2C

if __name__ == "__main__":

    env = diambra.arena.make("doapp", {"hardcore": True, "frame_shape": [128, 128, 1]})

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

#### Saving, loading and evaluating

```python
import diambra.arena
from stable_baselines3 import A2C
from stable_baselines3.common.evaluation import evaluate_policy

if __name__ == "__main__":

    # Create environment
    env = diambra.arena.make("doapp", {"hardcore": True, "frame_shape": [128, 128, 1]})

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

#### Parallel Environments

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
    print("Policy architecure:")
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

### Advanced

#### Dictionary Observations

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
    print("Policy architecure:")
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

#### Complete Training Script

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
    print("Policy architecure:")
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
