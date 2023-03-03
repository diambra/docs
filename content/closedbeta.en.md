---
title: Closed Beta Guide
disableToc: true
---

### How to Submit an Agent

<div style="font-size:1.125rem;">

- <a href="./#anatomy-of-an-agent-script-for-submission">Anatomy of an Agent script for submission</a>
- <a href="./#submit-a-pre-built-agent">Submit a pre-built Agent</a>
- <a href="./#submit-your-own-agent">Submit your own Agent</a>
- <a href="./#hide-your-source-code">Hide your source code</a>
- <a href="./#leverage-pre-built-dependencies-images">Leverage pre-built dependencies images</a>
- <a href="./#test-your-agent-before-submitting">Test your Agent before submitting</a>


</div>

{{% notice note %}}
Make sure you <a href="https://diambra.ai/register/" target="_blank">registered on our website</a>, correctly <a href="/#installation" target="_blank">installed DIAMBRA Arena</a> and run it at least once, so that your credentials are stored locally.
{{% /notice %}}

The basic process to submit an agent consists in three steps:
1. Writing a python script in which your agent interact with the environment exactly as if you were performing evaluation in your local machine
{{% notice tip %}}
In our <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a> we provide many examples, ranging from a trivial random agent to RL agents trained with state-of-the-art RL libraries.
{{% /notice %}}
2. Creating a docker image containing all dependencies needed to run such agent and push it to a **public** container registry
3. Submitting the docker image to the platform using our Command Line Interface

In what follows, we guide you through this process, starting from the easiest use case and building upon it to teach you how to leverage the most advanced features.


{{% notice warning %}}
Currently, we can only process Docker images built for amd64 CPU architecture. So, if you are using MacOS with M1 or M2 CPUs, you need to explicitly tell Docker to do that at build time as follows:<br><br>1. Open Docker Desktop dashboard / Preferences (cog icon) / Turn Experimental Features on & apply<br>2. Create a new builder instance with `docker buildx create --use`<br>3. Run `docker buildx build --platform linux/amd64 --push -t <image-tag> .`<br><br>Note that:<br>- If you can’t see an “Experimental Features” option, sign up for the <a href="https://www.docker.com/community/get-involved/developer-preview/" target="_blank">Docker developer program</a><br>- You have to push directly to a repository instead of doing it after build
{{% /notice %}}

#### Anatomy of an Agent script for submission

<figure style="margin-bottom:40px; margin-top:20px; margin-right:auto; margin-left:auto; width: 100%;">
  <img src="/images/agent.jpg" style="margin-top:0px;margin-bottom:20px; margin-right:0px; margin-left:0px;">
  <figcaption align="middle">Structure of an Agent script ready to be submitted</figcaption>
</figure>

The central element of a submission is the agent python script. Its structure is always composed by two main parts, highlighted in the picture above: the preparation step, where the agent and the environment setup is completed, and the interaction loop, where the classical agent-environment interaction happens. 

To prepare your agent for a submission on DIAMBRA platform, you just need to implement a classic loop as described above: after a first call to the reset method, you start iterating alternating the action selection step to the environment stepping instruction, resetting the environment when the episode is done. That's it, we take care of the rest. 

There is one thing that is worth noticing: since we want your submission to be the same no matter how many episodes are needed to evaluate it, you need to implement the while loop in a way that it continues to iterate (`while True:`), and you only exit (the `break` statement) when the value `info["env_done"]` is true. This value is set by us and used to let the agent know that the evaluation has been completed. In this way, the same script can be used to run evaluations made of 3, 5, 10 or whatever number of episodes you want, without changing a single line. 

#### Submit a pre-built Agent

In <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents repo</a>, together with different source code examples, we also provide <a href="https://github.com/orgs/diambra/packages?repo_name=agents" target="_blank">pre-built docker images (packages)</a> for some of them. For example, <a href="https://github.com/diambra/agents/pkgs/container/agent-random-1" target="_blank">here</a> you find the pre-built docker image for the random agent correspondent to <a href="https://github.com/diambra/agents/blob/main/basic/random_1/agent.py" target="_blank">this</a> source code.

As indicated by the python script settings, this random agent will play using a "Random" character in "Dead Or Alive++" game. 

Using this pre-built docker image you can easily perform your first submission ever on DIAMBRA platform, and appear in the official online leaderboard by simply typing in your preferred shell the following command:

```shell
diambra agent submit <docker image>
```
where instead of `<docker image>` you will use the `registry/name:tag` indicated at the top of the package page (i.e. `ghcr.io/diambra/agent-random-1:main`, making sure to use the latest available tag).

You will receive a confirmation of the submission, its identification number as well as the url where to see the results, something similar to the following:

```
diambra agent submit ghcr.io/diambra/agent-random-1:main
🖥️  (178) Agent submitted: https://diambra.ai/submission/178
```

{{% notice note %}}
For the closed beta, when pasting in your browser the url returned by the cli, remember to add `r.` before the domain, (in this case you would obtain `https://r.diambra.ai/submission/178`).
{{% /notice %}}

{{% notice note %}}
By default, the submission will select the lowest difficulty level (`"Easy"`) of the three available (`"Easy"`, `"Medium"`, `"Hard"`). To change this, you can add the `--submission.difficulty` argument to the previous command (e.g. `diambra agent submit <docker image> --submission.difficulty Medium`)
{{% /notice %}}

#### Submit your own Agent

Instead of using a pre-built image featuring a random agent, you can create your own. Our Command Line Interface allows you to start easily:

1. Generate the base files:

    ```shell
    diambra agent init .
    ```
    this command will generate the base random agent code (`agent.py`), the requirements with the essential dependencies (`requirements.txt`) and the Dockerfile to create a docker image with it (`Dockerfile`). So instead of writing your random agent from scratch, together with its dependencies list and the Dockerfile, you have a head start! (You could also leverage the agent we provide <a href="https://github.com/diambra/agents" target="_blank">here</a>)

2. Build the Docker image:

   ```shell
   docker build -t <registry>/<name>:<tag> .
   ```

   This will create the docker image and tag it. You can use any public registry, like <a href="https://quay.io" target="_blank">quay.io</a> or <a href="https://dockerhub.com" target="_blank">dockerhub,</a> but make sure the image is public. 

3. Push the image to the registry:

    ```shell
    docker push <registry>/<name>:<tag>
    ```

Once these steps are completed, you can submit the agent to the platform as shown above:

```shell
diambra agent submit <docker image>
```

where this time the `<docker image>` will be the image your just pushed, `<registry>/<name>:<tag>`.

#### Hide your source code

{{% notice note %}}
To hide your code you will need to use secret tokens, a common secure way to access private files via command line and APIs. Every hosting service has its specific procedure to generate them, we will use GitHub that provides clear guidance: <a href="https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic" target="_blank">Creating personal access token (GitHub).</a> **Make sure to tick the "repo" flag in the "select scopes" section.**
{{% /notice %}}

In the previous example, all your code has been added to the docker image that you pushed in a container registry and made public. It means that the code and data contained in it are accessible to everyone. 

If you want to avoid that, we got you covered. The process is very similar to the previous one:

1. Generate the base files with the `--secret` argument:

    ```shell
    diambra agent init --secret .
    ```
    this command will generate the same files as in the previous case, so the base random agent code (`agent.py`), the requirements (`requirements.txt`) and the Dockerfile (`Dockerfile`). But this time, there are two major differences:
    * The Docker file will not have the `COPY . .` instruction, as you don't want to copy in it your source code, nor the `ENTRYPOINT`. You will specify them both in the submission manifest (see next point)
    * A new yaml file is generated (`submission.yaml`), it is an example of the submission manifest, a file in which you can specify the different parameters of your submission. It contains things like the docker image to be used, the difficulty level, but most importantly:
      * The command to be executed inside the docker image
      * The source files to be downloaded and added to your docker image
      
    
    Point 3 below explains how to properly edit the submission manifest. 

2. Build the Docker image containing all your required dependencies and push it:

   ```shell
   docker build -t <registry>/<name>:<tag> .
   docker push <registry>/<name>:<tag>
   ```

3. Edit the submission manifest (`submission.yaml`) in the following fields:
   * `image`: indicating your docker image `<registry>/<name>:<tag>`
   * `command`: indicating the command you want to execute, in this example it consists of two lines `python` and `"/sources/agent.py"` as we want to run the `agent.py` python script
   * `sources`: adding the source, and the correspondent placeholder for the token, to retrieve the python script containing your agent

    Here is how the `submission.yaml` file should look like:

    ```yaml
    ---
    mode: AIvsCOM
    image: <registry>/<name>:<tag>
    command:
    - python
    - "/sources/agent.py"
    sources:
        agent.py: https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/random-agent/agent.py
    ```

{{% notice warning %}}
**Do not add your tokens directly in the submission YAML file, they will be publicly visible.**
{{% /notice %}}

Once these steps are completed, you can submit the agent to the platform as follows:

```shell
diambra agent submit --submission.manifest submission.yaml --submission.secret token=<my-secret token>
```

where `<my-secret token>` will be the token you created for your private hosting.

If, in addition to the agent script, you have other files you want to keep hidden, for example the weights of your neural network, you can easily extend the submission manifest. Let's say that your weights are a zip file that needs to be provided as input argument to your script, the `submission.yaml` would become:

```yaml
---
mode: AIvsCOM
image: <registry>/<name>:<tag>
command:
- python
- "/sources/agent.py"
- "/sources/model.zip"
sources:
    agent.py: https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/trained-agent/agent.py
    model.zip: https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/nn-weights/model.zip
```

If, for some reason, you need to organize your files in subfolders, just write the path you expect your files to have in the sources section of the submission manifest, and we will took care of the rest. If, for example, you want to store your models weights in a nested folder named "models", you would modify the previous submission manifest as follows:

```yaml
---
mode: AIvsCOM
image: <registry>/<name>:<tag>
command:
- python
- "/sources/agent.py"
- "/sources/models/model.zip"
sources:
    agent.py: https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/trained-agent/agent.py
    models/model.zip: https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/nn-weights/model.zip
```

#### Leverage pre-built dependencies images

If you are using the state of the art RL libraries we natively support (Stable Baselines, Stable Baselines 3 and Ray RL Lib) or simply need to use the DIAMBRA Arena base, instead of building your own image from scratch, you can directly leverage the pre-built ones we publicly provide! You can find all of them in the <a href="https://github.com/orgs/diambra/packages?repo_name=arena" target="_blank">Packages section</a> of the GitHub repo.

You can use them in at least two ways:
* Directly specifying them in your YAML submission file, in the `image` field (i.e. `image: ghcr.io/diambra/arena-base-on3.7-bullseye:main`)
* Building your own custom Docker image based on them, so using the `FROM` instruction in your Dockerfile (i.e. `FROM ghcr.io/diambra/arena-base-on3.7-bullseye:main`)

#### Test your Agent before submitting

If you want to test your agent locally before submitting it for evaluation on the platform, you can use the specific feature provided by our command line interface. The pattern of the command is the very same used for submission, except that instead of the `submit` option you will use `test`. 

It can be used to make sure the agent behaves as expected, and to debug it in case it fails, without waiting for the online evaluation pipeline.

It works with both plain docker images as well as submission manifests with privately hosted files and secret tokens, using respectively, the following commands:

```shell
diambra agent test <docker image>
```
or
```shell
diambra agent test --submission.manifest submission.yaml --submission.secret token=<my-secret token>
```