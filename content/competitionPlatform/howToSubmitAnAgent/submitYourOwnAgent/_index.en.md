---
title: Submit Your Own Agent
weight: 50
math: true
---

These are the steps to submit your own agent (we will use GitHub as an example):

1. Store your agent files (e.g. scripts and weights) in private repository
2. Create your personal access token (<a href="https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic" target="_blank">official docs here</a>):
   - Go to "Settings" in the top-right corner of the GitHub website.
   - Click "Developer settings" at the bottom-left of the page.
   - Click "Personal access tokens" and then "Generate new token."
   - Give your token a name, select the necessary scopes (e.g., "repo" for accessing private repositories), and click "Generate token."
   - Copy the generated token and save it somewhere safe, as you won't be able to see it again.
3. Submit your AI agent:
   - Choose the appropriate dependencies image for your submission. We provide <a href="https://github.com/orgs/diambra/packages?repo_name=arena" target="_blank">different pre-built ones</a> giving access to different common third party libraries
   - Submit your agent as shown in the following examples

#### Example 1: Command Line Interface Command

Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image and have your agent's files stored on GitHub:

```sh
diambra \
  --submission.mode AIvsCOM \
  --submission.source agent.py=https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/trained-agent/your_agent.py \
  --submission.source models/model.zip=https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/nn-weights/your_model.zip \
  --submission.secret token=your_gh_token \
  --submission.set-command \
  agent submit arena-stable-baselines3-on3.10-bullseye \
  python "/sources/agent.py" "/sources/models/model.zip"

```
  
Replace `your_gh_token`, `your_agent.py` and `your_model.zip` with the appropriate values.

The `--submission.source` flag takes `URL<->path/in/container` mappings to download files to the specified path, the `{{ .Secrets.<something> }}` can be used to include secrets specified in the `--submission.secret` flag. Combining both flags, you can create a submission that includes secrets and sources to download your weights from your private repository.

#### Example 2: Using a Manifest File

Alternatively, you can use a manifest file to define your submission. Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image and have your agent's files stored on GitHub, create a file named `submission-manifest.yaml` with the following content:

```yaml
mode: AIvsCOM
image: diambra/arena-stable-baselines3-on3.10-bullseye:main
command:
  - python
  - "/sources/agent.py"
  - "/sources/models/model.zip"
sources:
  agent.py: https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/trained-agent/your_agent.py
  models/model.zip: https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/nn-weights/your_model.zip
```

Replace `your_agent.py` and `your_model.zip` with the appropriate values.

Then, submit your agent using the manifest file:

```sh
diambra agent submit  --submission.secret token=your_gh_token --submission.manifest submission-manifest.yaml
```
  
Replace `your_gh_token` with the GitHub token you saved earlier.

{{% notice warning %}}
**Do not add your tokens directly in the submission YAML file, they will be publicly visible.**
{{% /notice %}}













{{% notice note %}}
To hide your code you will need to use secret tokens, a common secure way to access private files via command line and APIs. Every hosting service has its specific procedure to generate them, we will use GitHub that provides clear guidance: **Make sure to tick the "repo" flag in the "select scopes" section.**
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