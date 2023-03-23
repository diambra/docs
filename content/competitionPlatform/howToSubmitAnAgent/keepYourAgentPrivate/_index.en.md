---
title: Keep Your Agent Private
weight: 50
math: true
---

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
diambra agent submit  --submission.secret token=<my-secret token> --submission.manifest submission.yaml
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