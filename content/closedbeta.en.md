---
title: Closed Beta Guide
disableToc: true
---

### Index

<div style="font-size:1.125rem;">

- <a href="./#submit-an-agent">Submit an Agent</a>

</div>

## Submit an Agent

{{% notice note %}}
Make sure you <a href="https://diambra.ai/register/" target="_blank">registered on our website</a>, correctly <a href="/#installation" target="_blank">installed DIAMBRA Arena</a> and run it at least once, so that your credentials are stored locally.
{{% /notice %}}

Submitting an agent requires the following steps:
1. Write a python script in which your agent interact with the environment exactly as if you were performing evaluation in your local environment
{{% notice tip %}}
In our <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a> we provide many examples, ranging from a trivial random agent to a RL agent trained with Stable Baselines library.
{{% /notice %}}
2. Create a docker allowing to run such agent (i.e. featuring all dependencies it needs, etc) and push it to a **public** container registry
3. Submitting the created docker image to the platform using our Command Line Interface

In what follows, we guide you through this process, starting from the easiest use case and building upon it to teach you how to leverage the most advanced features.

#### Submit a pre-built Random Agent

In <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a>, together with different source code examples, we also provide <a href="https://github.com/orgs/diambra/packages?repo_name=agents" target="_blank">pre-built docker images (packages)</a> for some of them. For example, <a href="https://github.com/diambra/agents/pkgs/container/agent-random-1" target="_blank">here</a> you find the pre-build docker image for the random agent correspondent to <a href="https://github.com/diambra/agents/blob/main/basic/random_1/agent.py" target="_blank">this</a> source code.

As indicated by the python script settings, this random agent will play using a "Random" character in "Dead Or Alive++" game, at the highest difficulty level. 

Using this pre-build docker image you can easily perform your first ever submission on DIAMBRA platform and appear in the official online leaderboard by simply typing in your preferred shell the following command:

```shell
diambra agent submit <docker image>
```
where instead of `<docker image>` you will use the `name:tag` indicated at the top of the package page (i.e. `ghcr.io/diambra/agent-random-1:681c7768c51e5cbaa6e24ba5c026242cd2339037`, making sure to use the latest available tag).

#### Submit your own Random Agent

#### Submit your own Random Agent hiding your source code


```
mkdir agent
cd agent
diambra agent ini .
docker build -t foo/bar .
docker push foo/bar
diambra agent submit --manifest submission.yaml -secret token=<my-secret token>
```

I think this should already print out a url to watch the submission status. If not, fill me an GH issue. The `--secret` stuff is only needed if you reference secrets in `manifest.yaml`.