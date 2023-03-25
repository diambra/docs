---
title: How To Submit An Agent
weight: 30
math: true
---


The basic process to submit an agent consists in three steps:
1. Writing a python script in which your agent interact with the environment exactly as if you were performing evaluation in your local machine
{{% notice tip %}}
In our <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a> we provide many examples, ranging from a trivial random agent to RL agents trained with state-of-the-art RL libraries.
{{% /notice %}}
2. Creating a docker image that contains the agent script and all its dependencies, and pushing it to a **public** container registry
3. Submitting the docker image to the platform using our Command Line Interface
{{% notice note %}}
If you do not specify any tag when submitting a Docker image, the tag `latest` will be used by default. If that tag is not present, the submission will fail.
{{% /notice %}}

In the subsections linked below, we guide you through this process, starting from the easiest use case and building upon it to show you how to leverage the most advanced features.

<div style="font-size:1.125rem;">

- <a href="./submitprebuiltagents/">Submit pre-built Agents</a>
- <a href="./submityourownagent/">Submit your own Agent</a>
- <a href="./keepyouragentprivate/">Keep your Agent private</a>
</div>

{{% notice note %}}
You need to be <a href="https://diambra.ai/register/" target="_blank">registered on our website</a> in order to submit your agent.
{{% /notice %}}

{{% notice warning %}}
Currently, we can only process Docker images built for amd64 CPU architecture. So, if you are using MacOS with M1 or M2 CPUs, you need to explicitly tell Docker to do that at build time as follows:<br><br>1. Open Docker Desktop Dashboard / Preferences (cog icon) / Turn "Experimental Features" on & apply<br>2. Create a new builder instance with `docker buildx create --use`<br>3. Run `docker buildx build --platform linux/amd64 --push -t <image-tag> .`<br><br>Note that:<br>- If you can’t see an “Experimental Features” option, sign up for the <a href="https://www.docker.com/community/get-involved/developer-preview/" target="_blank">Docker developer program</a><br>- You have to push directly to a repository instead of doing it after build
{{% /notice %}}