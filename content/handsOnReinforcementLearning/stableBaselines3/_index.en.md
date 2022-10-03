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

This page aims at guiding the user, who is expected to be at least familiar with the basic concepts of RL ([see Learning RL page](/deeprltraining/learningrl/)), in a step-by-step process that will make him able to train a state-of-the-art DeepRL agent capable of obtaining good performances in our environments.

### Getting Ready

```shell
conda create -n diambra-arena-sb3 python=3.8
```

```shell
pip install stable-baselines3[extra]
pip install gym[all]
pip install diambra-arena
```

<a href="https://stable-baselines3.readthedocs.io/en/master/guide/install.html" target="_blank">Stable-Baselines3 Installation Docs</a>

If one wants to rely on already implemented RL algorithms, focusing his efforts on higher level aspects such as policy network architecture, features selection, hyper-parameters tuning, and so on, the best choice is to leverage state-of-the-art RL libraries as the ones shown below. There are many different options, here we list those that, in our experience, are recognized as the leaders in the field, and have been proven to achieve good performances in DIAMBRA Arena environments.

There are multiple advantages related to the use of these libraries, to name a few: they provide high quality RL algorithms, efficiently implemented and continuously tested, they allow to natively parallelize environment execution, and in some cases they even support distributed training using multiple GPUs in a single workstation or even in cluster contexts.

We will provide guidance and examples using some of the options listed down here.

### Basic

#### Basic Example

```python
import diambra.arena
from stable_baselines3 import A2C

env = diambra.arena.make("doapp", {"hardcore": True, "frame_shape": [128, 128, 1]})

model = A2C('CnnPolicy', env, verbose=1)
model.learn(total_timesteps=1000)

observation = env.reset()
while True:
    env.render()

    action, _state = model.predict(observation, deterministic=True)

    observation, reward, done, info = env.step(action)

    if done:
        observation = env.reset()
        break

env.close()
```

#### Saving, loading and evaluating

```python
import diambra.arena
from stable_baselines3 import A2C
from stable_baselines3.common.evaluation import evaluate_policy

# Create environment
env = diambra.arena.make("doapp", {"hardcore": True, "frame_shape": [128, 128, 1]})

# Instantiate the agent
model = A2C('CnnPolicy', env, verbose=1)
# Train the agent
model.learn(total_timesteps=1000)
# Save the agent
model.save("a2c_doapp")
del model  # delete trained model to demonstrate loading

# Load the trained agent
# NOTE: if you have loading issue, you can pass `print_system_info=True`
# to compare the system on which the model was trained vs the current one
# model = A2C.load("a2c_doapp", env=env, print_system_info=True)
model = A2C.load("a2c_doapp", env=env)

# Evaluate the agent
# NOTE: If you use wrappers with your environment that modify rewards,
#       this will be reflected here. To evaluate with original rewards,
#       wrap environment in a "Monitor" wrapper before other wrappers.
mean_reward, std_reward = evaluate_policy(model, model.get_env(), n_eval_episodes=3)
print("Reward: {} (avg) ± {} (std)".format(mean_reward, std_reward))

# Enjoy trained agent
observation = env.reset()
cumulative_reward = 0
while True:
    env.render()

    action, _state = model.predict(observation, deterministic=True)

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
import os
import sys
import diambra.arena

from stable_baselines3 import PPO
from stable_baselines3.common.vec_env import DummyVecEnv, SubprocVecEnv, VecFrameStack
from stable_baselines3.common.utils import set_random_seed

# Make Stable Baselines Env function
def make_stable_baselines_env(game_id, env_settings, wrappers_settings=None,
                              frame_stack=1, use_subprocess=True, seed=0):
    """
    Create a wrapped VecEnv.
    :param game_id: (str) the game environment ID
    :param env_settings: (dict) parameters for DIAMBRA Arena environment
    :param wrappers_settings: (dict) parameters for environment
                              wraping function
    :param use_subprocess: (bool) Whether to use `SubprocVecEnv` or
                          `DummyVecEnv` when
    :param no_vec: (bool) Whether to avoid usage of Vectorized Env or not.
                   Default: False
    :param seed: (int) initial seed for RNG
    :return: (VecEnv) The diambra environment
    """

    env_addresses = os.getenv("DIAMBRA_ENVS", "").split()
    if len(env_addresses) == 0:
        raise Exception("ERROR: Running script without DIAMBRA CLI.")
        sys.exit(1)

    num_envs = len(env_addresses)

    def make_sb_env(rank):
        def _init():
            env = diambra.arena.make(game_id, env_settings, wrappers_settings,
                                     seed=seed + rank, rank=rank)
            return env
        return _init
    set_random_seed(seed)

    # When using one environment, no need to start subprocesses
    if num_envs == 1 or not use_subprocess:
        env = DummyVecEnv([make_sb_env(i) for i in range(num_envs)])
    else:
        env = SubprocVecEnv([make_sb_env(i) for i in range(num_envs)])

    # Stack frames
    if (frame_stack > 1):
        env = VecFrameStack(env, n_stack=frame_stack, channels_order="last")

    return env, num_envs

if __name__ == '__main__':

    # Settings
    settings = {}
    settings["hardcore"] = True
    settings["frame_shape"] = [128, 128, 1]
    settings["characters"] = [["Kasumi"], ["Kasumi"]]

    # Wrappers Settings
    wrappers_settings = {"reward_normalization": True}

    # Create environment
    env, num_envs = make_stable_baselines_env("doapp", settings, wrappers_settings, frame_stack=4)
    print("Activated {} environment(s)".format(num_envs))

    print("Observation space shape =", env.observation_space.shape)
    print("Observation space type =", env.observation_space.dtype)

    print("Act_space =", env.action_space)

    # Instantiate the agent
    model = PPO('CnnPolicy', env, verbose=1)
    # Train the agent
    model.learn(total_timesteps=1000)

    # Enjoy trained agent
    observation = env.reset()
    cumulative_reward = [0.0 for _ in range(num_envs)]
    while True:
        env.render()

        action, _state = model.predict(observation, deterministic=True)

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
