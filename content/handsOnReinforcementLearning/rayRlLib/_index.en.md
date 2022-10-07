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

### Getting Ready

We highly recommend using virtual environments to isolate your python installs, especially to avoid conflicts in dependencies. In what follows we use Conda but any other tool should work too.

Create and activate a new dedicated virtual environment:

```shell
conda create -n diambra-arena-ray python=3.8
conda activate diambra-arena-ray
```

Install DIAMBRA Arena with Stable Baselines 3 interface:

```shell
pip install diambra-arena[ray-rllib]
```

This should be enough to prepare your system to execute the following examples. You can refer to the official <a href="https://docs.ray.io/en/latest/rllib/index.html" target="_blank">Ray RLlib documentation</a> or reach out on our <a href="https://discord.gg/tFDS2UN5sv" target="_blank">Discord server</a> for specific needs.

All the examples presented below are available here: <a href="https://github.com/diambra/agents/tree/main/ray_rllib" target="_blank">DIAMBRA Agents - Ray RLlib</a>. They have been created following the high level approach found on <a href="https://docs.ray.io/en/latest/rllib/rllib-examples.html" target="_blank">Ray RLlib examples</a> page and their related <a href="https://github.com/ray-project/ray/tree/master/rllib/examples" target="_blank">repository collection</a>, thus allowing to easily extend them and to understand how they interface with the different components.

### Basic

#### Basic Example

```python
import diambra.arena
from diambra.arena.ray_rllib.make_ray_env import DiambraArena, preprocess_ray_config
from ray.rllib.algorithms.ppo import PPO

if __name__ == "__main__":

    # Settings
    settings = {}
    settings["hardcore"] = True
    settings["frame_shape"] = [84, 84, 1]

    config = {
        # Define and configure the environment
        "env": DiambraArena,
        "env_config": {
            "game_id": "doapp",
            "settings": settings,
        },
        "num_workers": 0,
        "train_batch_size": 200,
    }

    # Update config file
    config = preprocess_ray_config(config)

    # Create the RLlib Agent.
    agent = PPO(config=config)

    # Run it for n training iterations
    print("\nStarting training ...\n")
    for idx in range(1):
        print("Training iteration:", idx + 1)
        agent.train()
    print("\n .. training completed.")

    # Run the trained agent (and render each timestep output).
    print("\nStarting trained agent execution ...\n")

    env = diambra.arena.make("doapp", settings)

    obs = env.reset()
    while True:
        env.render()

        action = agent.compute_single_action(obs)

        obs, reward, done, info = env.step(action)

        if done:
            obs = env.reset()
            break

    print("\n... trained agent execution completed.\n")

    env.close()
```

#### Saving, loading and evaluating

```python
import diambra.arena
from diambra.arena.ray_rllib.make_ray_env import DiambraArena, preprocess_ray_config
from ray.rllib.algorithms.ppo import PPO
from ray.tune.logger import pretty_print

if __name__ == "__main__":

    # Settings
    settings = {}
    settings["hardcore"] = True
    settings["frame_shape"] = [84, 84, 1]

    config = {
        # Define and configure the environment
        "env": DiambraArena,
        "env_config": {
            "game_id": "doapp",
            "settings": settings,
        },
        "num_workers": 0,
        "train_batch_size": 200,
        "framework": "torch",
    }

    # Update config file
    config = preprocess_ray_config(config)

    # Create the RLlib Agent.
    agent = PPO(config=config)
    print("Policy architecture =\n{}".format(agent.get_policy().model))

    # Run it for n training iterations
    print("\nStarting training ...\n")
    for idx in range(1):
        print("Training iteration:", idx + 1)
        results = agent.train()
    print("\n .. training completed.")
    print("Training results:\n{}".format(pretty_print(results)))

    # Save the agent
    checkpoint = agent.save()
    print("Checkpoint saved at {}".format(checkpoint))
    del agent  # delete trained model to demonstrate loading

    # Load the trained agent
    agent = PPO(config=config)
    agent.restore(checkpoint)
    print("Agent loaded")

    # Evaluate the trained agent (and render each timestep to the shell's
    # output).
    print("\nStarting evaluation ...\n")
    results = agent.evaluate()
    print("\n... evaluation completed.\n")
    print("Evaluation results:\n{}".format(pretty_print(results)))
```

#### Parallel Environments

```python
import diambra.arena
from diambra.arena.ray_rllib.make_ray_env import DiambraArena, preprocess_ray_config
from ray.rllib.algorithms.ppo import PPO
from ray.tune.logger import pretty_print

if __name__ == "__main__":

    # Settings
    settings = {}
    settings["hardcore"] = True
    settings["frame_shape"] = [84, 84, 3]

    config = {
        # Define and configure the environment
        "env": DiambraArena,
        "env_config": {
            "game_id": "doapp",
            "settings": settings,
        },

        "train_batch_size": 200,

        # Use 2 rollout workers
        "num_workers": 2,
        # Use a vectorized env with 2 sub-envs.
        "num_envs_per_worker": 2,

        # Evaluate once per training iteration.
        "evaluation_interval": 1,
        # Run evaluation on (at least) two episodes
        "evaluation_duration": 2,
        # ... using one evaluation worker (setting this to 0 will cause
        # evaluation to run on the local evaluation worker, blocking
        # training until evaluation is done).
        "evaluation_num_workers": 1,
        # Special evaluation config. Keys specified here will override
        # the same keys in the main config, but only for evaluation.
        "evaluation_config": {
            # Render the env while evaluating.
            # Note that this will always only render the 1st RolloutWorker's
            # env and only the 1st sub-env in a vectorized env.
            "render_env": True,
        },
    }

    # Update config file
    config = preprocess_ray_config(config)

    # Create the RLlib Agent.
    agent = PPO(config=config)

    # Run it for n training iterations
    print("\nStarting training ...\n")
    for idx in range(2):
        print("Training iteration:", idx + 1)
        results = agent.train()
    print("\n .. training completed.")
    print("Training results:\n{}".format(pretty_print(results)))
```

### Advanced

#### Dictionary Observations

```python
import diambra.arena
from diambra.arena.ray_rllib.make_ray_env import DiambraArena, preprocess_ray_config
from ray.rllib.algorithms.ppo import PPO
from ray.tune.logger import pretty_print

if __name__ == "__main__":

    # Settings
    settings = {}
    settings["frame_shape"] = [84, 84, 1]
    settings["characters"] = [["Kasumi"], ["Kasumi"]]

    # Wrappers Settings
    wrappers_settings = {}
    wrappers_settings["reward_normalization"] = True
    wrappers_settings["actions_stack"] = 12
    wrappers_settings["frame_stack"] = 5
    wrappers_settings["scale"] = True
    wrappers_settings["process_discrete_binary"] = True

    config = {
        # Define and configure the environment
        "env": DiambraArena,
        "env_config": {
            "game_id": "doapp",
            "settings": settings,
            "wrappers_settings": wrappers_settings,
        },
        "num_workers": 0,
        "train_batch_size": 200,
        "framework": "torch",
    }

    # Update config file
    config = preprocess_ray_config(config)

    # Create the RLlib Agent.
    agent = PPO(config=config)
    print("Policy architecture =\n{}".format(agent.get_policy().model))

    # Run it for n training iterations
    print("\nStarting training ...\n")
    for idx in range(1):
        print("Training iteration:", idx + 1)
        results = agent.train()
    print("\n .. training completed.")
    print("Training results:\n{}".format(pretty_print(results)))

    # Evaluate the trained agent (and render each timestep to the shell's
    # output).
    print("\nStarting evaluation ...\n")
    results = agent.evaluate()
    print("\n... evaluation completed.\n")
    print("Evaluation results:\n{}".format(pretty_print(results)))
```
