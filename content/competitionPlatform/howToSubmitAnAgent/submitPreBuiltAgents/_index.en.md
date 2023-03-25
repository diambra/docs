---
title: Submit Pre-Built Agents
weight: 30
math: true
---

In <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a>, together with different source code examples, we also provide <a href="https://github.com/orgs/diambra/packages?repo_name=agents" target="_blank">pre-built docker images (packages)</a> for some of them. For example, <a href="https://github.com/diambra/agents/pkgs/container/agent-random-1" target="_blank">here</a> you find the pre-built docker image for the random agent correspondent to <a href="https://github.com/diambra/agents/blob/main/basic/random_1/agent.py" target="_blank">this</a> source code.

As indicated by the python script settings, this random agent will play using a "Random" character in a random game. 

Using this pre-built docker image you can easily perform your first submission ever on DIAMBRA platform, and appear in the official online leaderboard by simply typing in your preferred shell the following command:

```shell
diambra agent submit <docker image>
```
where instead of `<docker image>` you will use the `registry/name:tag` indicated at the top of the package page (i.e. `ghcr.io/diambra/agent-random-1:main`, making sure to use the latest available tag).

{{% notice note %}}
By default, the random agent will randomly select the game on which to run. If you want to specify it, use the `--gameId` command line argument that our pre-built image accepts, leveraging the command line interface when submitting the docker image as follows: `diambra agent submit <docker image> "--gameId" "tektagt"`. Additional similar use cases are covered in the <a href="./#append-args-or-override-submissions-entrypoint-via-cli">"Append args or override submissions entrypoint via CLI"</a> section below.
{{% /notice %}}

After running the command, you will receive a submission confirmation, its identification number as well as the url where to see the results, something similar to the following:

```
diambra agent submit ghcr.io/diambra/agent-random-1:main
🖥️  (178) Agent submitted: https://diambra.ai/submission/178
```

{{% notice note %}}
By default, the submission will select the lowest difficulty level (`"Easy"`) of the three available (`"Easy"`, `"Medium"`, `"Hard"`). To change this, you can add the `--submission.difficulty` argument to the previous command (e.g. `diambra agent submit --submission.difficulty Medium <docker image>`)
{{% /notice %}}