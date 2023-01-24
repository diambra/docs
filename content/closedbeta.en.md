---
title: Closed Beta Guide
disableToc: true
---

### Index

<div style="font-size:1.125rem;">

- <a href="./#submit-an-agent">Submit an Agent</a>

</div>


Welcome Vanguard Team!

## Submit an Agent

{{% notice note %}}
Make sure you <a href="https://diambra.ai/register/" target="_blank">registered on our website</a>, correctly <a href="/#installation" target="_blank">installed DIAMBRA Arena</a> and run it at least once, so that your credentials are stored locally.
{{% /notice %}}

Submitting an agent requires the following steps:
1. Writing a python script in which your agent interact with the environment exactly as if you were performing evaluation in your local machine
{{% notice tip %}}
In our <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a> we provide many examples, ranging from a trivial random agent to RL agents trained with state-of-the-art RL libraries.
{{% /notice %}}
2. Creating a docker image containing all dependencies needed to run such agent and push it to a **public** container registry
3. Submitting the created docker image to the platform using our Command Line Interface

In what follows, we guide you through this process, starting from the easiest use case and building upon it to teach you how to leverage the most advanced features.

#### Submit a pre-built Random Agent

In <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a>, together with different source code examples, we also provide <a href="https://github.com/orgs/diambra/packages?repo_name=agents" target="_blank">pre-built docker images (packages)</a> for some of them. For example, <a href="https://github.com/diambra/agents/pkgs/container/agent-random-1" target="_blank">here</a> you find the pre-built docker image for the random agent correspondent to <a href="https://github.com/diambra/agents/blob/main/basic/random_1/agent.py" target="_blank">this</a> source code.

As indicated by the python script settings, this random agent will play using a "Random" character in "Dead Or Alive++" game, at the highest difficulty level. 

Using this pre-built docker image you can easily perform your first submission ever on DIAMBRA platform, and appear in the official online leaderboard by simply typing in your preferred shell the following command:

```shell
diambra agent submit <docker image>
```
where instead of `<docker image>` you will use the `name:tag` indicated at the top of the package page (i.e. `ghcr.io/diambra/agent-random-1:681c7768c51e5cbaa6e24ba5c026242cd2339037`, making sure to use the latest available tag).

You will receive a confirmation of the submission, its identification number as well as the url where to see the results, something similar to the following:

```
diambra agent submit ghcr.io/diambra/agent-random-1:681c7768c51e5cbaa6e24ba5c026242cd2339037
🖥️  (178) Agent submitted: https://diambra.ai/submission/178
```

{{% notice note %}}
Note that, for the closed beta version the previous url needs to be manually tweaked, adding `r.` before the domain, thus becoming `https://r.diambra.ai/submission/178`
{{% /notice %}}

#### Submit your own Random Agent

If instead of using a pre-built image featuring a random agent, you can create your own, taking advantage of our Command Line Interface once again:

1. Generate the base files:

    ```shell
    diambra agent init .
    ```
    this command will generate the base random agent code (`agent.py`), the requirements with the essential dependencies (`requirements.txt`) and the Dockerfile to create a docker image with it (`Dockerfile`). So instead of writing your random agent from scratch, together with its dependencies list and the Dockerfile, you have a hot start! (You could also leverage the agent we provide <a href="https://github.com/diambra/agents" target="_blank">here</a>)

2. Build the Docker image:

   ```shell
   docker build -t <registry>/<image> .
   ```

   This will create the docker image and tag it. You can use any public registry, like <a href="https://quay.io" target="_blank">quay.io</a> or <a href="https://dockerhub.com" target="_blank">dockerhub</a>, but make sure the image is public. 

3. Push the image to the registry:

    ```shell
    docker push <registry>/<image>
    ```

Once these steps are completed, you can submit the agent to the platform as shown above:

```shell
diambra agent submit <docker image>
```

where this time the `<docker image>` will be the image your just pushed, `<registry>/<image>`.

#### Submit your own Random Agent hiding your source code

In the previous example, all your code has been added to the docker image that you pushed in a container registry and made public. It means that the code and data contained in it are accessible to everyone. 

```
mkdir agent
cd agent
diambra agent ini .
docker build -t foo/bar .
docker push foo/bar
diambra agent submit --manifest submission.yaml -secret token=<my-secret token>
```

I think this should already print out a url to watch the submission status. If not, fill me an GH issue. The `--secret` stuff is only needed if you reference secrets in `manifest.yaml`.