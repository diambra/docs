---
title: Ray RLlib
weight: 20
---

<div style="font-size:1.125rem;">

### Index

- <a href="./#getting-ready">Getting Ready</a>
- <a href="./#basic">Basic</a>
- <a href="./#advanced">Advanced</a>

</div>

This page aims at guiding the user, who is expected to be at least familiar with the basic concepts of RL ([see Learning RL page](/deeprltraining/learningrl/)), in a step-by-step process that will make him able to train a state-of-the-art DeepRL agent capable of obtaining good performances in our environments.

### Getting Ready

```shell
conda create -n diambra-arena-ray python=3.8
conda activate diambra-arena-ray
```

```shell
pip install diambra-arena[ray-rllib]
```

<a href="https://stable-baselines3.readthedocs.io/en/master/guide/install.html" target="_blank">Stable-Baselines3 Installation Docs</a>

If one wants to rely on already implemented RL algorithms, focusing his efforts on higher level aspects such as policy network architecture, features selection, hyper-parameters tuning, and so on, the best choice is to leverage state-of-the-art RL libraries as the ones shown below. There are many different options, here we list those that, in our experience, are recognized as the leaders in the field, and have been proven to achieve good performances in DIAMBRA Arena environments.

There are multiple advantages related to the use of these libraries, to name a few: they provide high quality RL algorithms, efficiently implemented and continuously tested, they allow to natively parallelize environment execution, and in some cases they even support distributed training using multiple GPUs in a single workstation or even in cluster contexts.

We will provide guidance and examples using some of the options listed down here.

<a href="https://github.com/diambra/agents/tree/main/stable_baselines3" target="_blank">DIAMBRA Agents Repo - Stable Baselines3 Examples</a>

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
