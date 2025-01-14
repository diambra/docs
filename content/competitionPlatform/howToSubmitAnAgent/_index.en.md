---
title: How To Submit An Agent
weight: 30
math: true
---

The basic process to submit an agent consists in the following steps:

1. Write a python script in which your agent interact with the environment exactly as if you were performing evaluation in your local machine
2. Store your trained agent's scripts and weights (if any) in a private repository and create a personal access token to the repository (in our examples we will use Hugging Face and GitHub).
3. Submit the agent using our Command Line Interface specifying your private repository files path, your secret token and one of the public pre-built dependencies images we provide

{{% notice tip %}}
In our <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a> we provide many examples, ranging from a trivial random agent to RL agents trained with state-of-the-art RL libraries.
{{% /notice %}}

In the subsections linked below, we guide you through this process, starting from the easiest use case and building upon it to show you how to leverage the most advanced features.

<div style="font-size:1.125rem;">

- <a href="./submitprebuiltagents/">Submit pre-built Agents</a>
- <a href="./submityourownagent/">Submit your own Agent</a>
- <a href="./customdependenciesimage/">Custom dependencies image</a>
</div>