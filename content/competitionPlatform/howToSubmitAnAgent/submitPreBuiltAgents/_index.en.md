---
title: Submit Pre-Built Agents
weight: 30
math: true
---

In <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a>, together with different source code examples, we also provide <a href="https://github.com/orgs/diambra/packages?repo_name=agents" target="_blank">pre-built docker images (packages)</a> for some of them. 

For example, <a href="https://github.com/diambra/agents/pkgs/container/agent-random-1" target="_blank">here</a> you find the pre-built docker image for the random agent correspondent to <a href="https://github.com/diambra/agents/blob/main/basic/random_1/agent.py" target="_blank">this</a> source code. As indicated by the python script settings, this random agent will play using a "Random" character in a random game. 

Using this pre-built docker image you can easily perform your first submission ever on DIAMBRA platform, and appear in the official online leaderboard by simply typing in your preferred shell the following command:

```shell
diambra agent submit diambra/agent-random-1:main
```
{{% notice note %}}
If you want to specify the game on which to run the random agent, use the `--gameId` command line argument that our pre-built image accepts, when submitting the docker image as follows: `diambra agent submit --gameId tektagt diambra/agent-random-1:main`. Additional similar use cases are covered in the <a href="../../argumentsandcommands/">"Arguments and Commands"</a> page.
{{% /notice %}}

After running the command, you will receive a submission confirmation, its identification number as well as the url where to see the results, something similar to the following:

```
diambra agent submit diambra/agent-random-1:main
🖥️  (178) Agent submitted: https://diambra.ai/submission/178
```

{{% notice note %}}
By default, the submission will select the lowest difficulty level (`Easy`) of the three available (`Easy`, `Medium`, `Hard`). To change this, you can add the `--submission.difficulty` argument: `diambra agent submit --submission.difficulty Medium diambra/agent-random-1:main`
{{% /notice %}}


{{% notice warning %}}
As shown here, it is possible to embed your agent files (i.e. scripts and weights) in the dependencies docker image and submit only that. Keep in mind that this image needs to be public and will be visible on the platform, so every user will be able to use it for his own submissions.
{{% /notice %}}
