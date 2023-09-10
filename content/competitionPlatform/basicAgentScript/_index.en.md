---
title: Basic Agent Script
weight: 10
math: true
---

The central element of a submission is the agent python script. Its structure is always composed by two main parts, highlighted in the snippet below: the preparation step, where the agent and the environment setup is completed, and the interaction loop, where the classical agent-environment interaction happens.

After a first call to the reset method, you start iterating alternating action selection and environment stepping, resetting the environment when the episode is done. That's it, we take care of the rest.

There is one thing that is worth noticing: since we want your submission to be the same no matter how many episodes are needed to evaluate it, you need to implement the while loop in a way that it keeps iterating indefinitely (`while True:`) and only exits (the `break` statement) when the value `info["env_done"]` is true. This value is set by us and used to let the agent know that the evaluation has been completed. In this way, the same script can be used to run evaluations made of 3, 5, 10 or whatever number of episodes you want, without changing a single line.

```python
import diambra.arena

if __name__ == "__main__":
    # Environment configuration
    settings = {
        "n_players": 1,
        "step_ratio": 6,
        "frame_shape": (128, 128, 1),
        "characters": ("Kasumi"),
        "action_space": "multi_discrete",
    }

    env = diambra.arena.make("doapp", settings)

    # Agent-environment interaction loop
    observation, info = env.reset()
    while True:
        action = env.action_space.sample()

        observation, reward, terminated, truncated, info = env.step(action)

        if terminated or truncated:
            observation, info = env.reset()
            if info["env_done"]:
                break

    env.close()
```