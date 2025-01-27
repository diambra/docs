---
title: Submit Your Own Agent
weight: 50
math: true
---

We support all types of git sources and we also include Hugging Face libraries for model download in the base image. Here’s a streamlined method to create, train, and submit your agent locally using the DIAMBRA CLI, followed by the recommended hosting options:

<div style="font-size:1.125rem;">

- <a href="./#local-cli">🖥️ Local CLI</a>
- <a href="./#-hugging-face">🤗 Hugging Face</a>
- <a href="./#github"><img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" alt="GitHub" style="width:20px; vertical-align:sub;"> GitHub</a>

</div>

{{% notice tip %}}
To favor an easy start, we provide example agent files (scripts and weights) that work out-of-the-box (but are only minimally trained) in our <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents</a> repository, for both <a href="https://github.com/diambra/agents/tree/main/stable_baselines3" target="_blank">Stable Baselines 3</a> and <a href="https://github.com/diambra/agents/tree/main/ray_rllib" target="_blank">Ray RLlib.</a>
{{% /notice %}}

### 🖥️ Local CLI

This method uses the DIAMBRA CLI to generate, build, and submit an agent directly from your local machine.

#### Steps to Submit Your Agent

1. **Initialize the Agent**:
   Run the `diambra agent init` command, specifying the path to the directory where your model is located (not the model itself). This will generate the required files for your agent.

   ```bash
   diambra agent init path/to/agent
   ```

   Example:
   ```bash
   diambra agent init ./output/models/
   ```

   This command creates the following files:
   - `agent.py`: A sample script for your agent.
   - `requirements.txt`: Lists the dependencies for your agent.
   - `Dockerfile`: Defines how to build the container for your agent.
   - `README.md`: A quick reference for your agent.

2. **Customize the `agent.py` Script**:
   Replace the sample logic in the `agent.py` file with your own pretrained agent logic. Here’s an example of how the script should look:

   ```python
   import diambra.arena
   from stable_baselines3 import PPO  # Import your RL framework

   # Model path and game ID
   MODEL_PATH = "./model.zip"
   GAME_ID = "doapp"

   # Load the trained agent
   agent = PPO.load(MODEL_PATH)

   # Environment settings setup and environment creation
   env = diambra.arena.make(GAME_ID)

   # Agent-Environment loop
   obs, info = env.reset()

   while True:
       # Predict the next action using the trained agent
       action, _ = agent.predict(obs, deterministic=True)

       obs, reward, terminated, truncated, info = env.step(action)

       if terminated or truncated:
           obs, info = env.reset()
           if info["env_done"]:
               break

   # Close the environment
   env.close()
   ```

3. **Update the `Dockerfile` and `requirements.txt` (If Necessary)**:
   If your agent requires additional dependencies or custom setups, update the following files:

   - **`requirements.txt`**:
     Add any Python packages your agent relies on. For example:
     ```plaintext
     diambra-arena==2.2.7
     stable-baselines3
     torch
     numpy
     ```

   - **`Dockerfile`**:
     Ensure the Dockerfile installs all required dependencies. If you add new packages to `requirements.txt`, no further changes are needed unless specific system-level dependencies are required.

     Example:
     ```dockerfile
     FROM ghcr.io/diambra/arena-base-on3.10-bullseye:main

     RUN apt-get -qy update && \
         apt-get -qy install libgl1 && \
         rm -rf /var/lib/apt/lists/*

     WORKDIR /app
     COPY requirements.txt .
     RUN pip install -r requirements.txt

     COPY . .
     ENTRYPOINT [ "python", "/app/agent.py" ]
     ```

4. **Submit Your Agent**:
   Submit your agent directly from the directory where the files are located. The DIAMBRA CLI will build and push the directory to DIAMBRA’s registry automatically.

   ```bash
   diambra agent submit .
   ```

5. **Track Your Submission**:
   After submission, you’ll receive a link to monitor your agent’s evaluation. For example:
   ```bash
   🖥️  logged in
   ...
   🖥️  (####) Agent submitted: https://diambra.ai/submission/####
   ```
   Visit the link to review your agent’s progress and results.

{{% notice warning %}}
<span style="color:#333333; font-weight:bolder;">Do not add your tokens directly in the submission YAML file, as they will be publicly visible.</span>
{{% /notice %}}

### 🤗 Hugging Face

These are the steps to submit your own agent hosted on Hugging Face:

1. Store your agent files (e.g. scripts and weights) in a private model
2. Create your personal access token (<a href="https://huggingface.co/docs/hub/security-tokens" target="_blank">official docs here</a>):
   - Log into the Hugging Face platform with your credentials.
   - Click on your profile image and go to "Settings".
   - On the left side bar menu, click the "Access Token" option.
   - Click on "New token" to create a new token, give it a name and assign it the "Read" permissions
   - Give your token a name, select the necessary scopes (e.g., "repo" for accessing private repositories), and click "Generate token."
   - Store the generated token in your local machine (<a href="https://huggingface.co/docs/huggingface_hub/en/quick-start#login-command" target="_blank">official docs here</a>):
     - Install the Hugging Face hub library: `pip install -U huggingface_hub`
     - From the terminal run the `login()` command: `huggingface-cli login`, which will tell you if you are already logged in and prompt you for your token. The token is then validated and saved in your `HF_HOME` directory (defaults to `~/.cache/huggingface/token`).
3. Submit your AI agent:
   - Choose the appropriate dependencies docker image for your submission. We provide <a href="https://github.com/orgs/diambra/packages?repo_name=arena" target="_blank">different pre-built ones</a> giving access to various common third party libraries
   - Submit your agent as shown in the following examples

#### Example 1: Using a Manifest File (Recommended)

Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image, create a file named `submission-manifest.yaml` with the following content:

```yaml
mode: AIvsCOM
image: diambra/arena-stable-baselines3-on3.10-bullseye:main
command:
  - python
  - "/sources/agent.py"
  - "/sources/models/model.zip"
sources:
  .: git+https://username:{{.Secrets.hf_token}}@huggingface.co/username/repository_name.git#ref=branch_name
```

Replace `username` and `repository_name.git#ref=branch_name` with the appropriate values, and change `image` and `command` fields according to your specific use case.

Then, submit your agent using the manifest file:

```sh
diambra agent submit --submission.secrets-from=huggingface --submission.manifest submission-manifest.yaml
```

This will automatically retrieve the Hugging Face token you saved earlier.

#### Example 2: Command Line Interface Only

If you want to avoid using submission files, you can use the command line to directly submit your agent. Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image:

```sh
diambra agent submit \
  --submission.mode AIvsCOM \
  --submission.source .=git+https://username:{{.Secrets.hf_token}}@huggingface.co/username/repository_name.git#ref=branch_name \
  --submission.secrets-from=huggingface \
  --submission.set-command \
  arena-stable-baselines3-on3.10-bullseye \
  python "/sources/agent.py" "/sources/models/model.zip"
```

Replace `username` and `repository_name.git#ref=branch_name` with the appropriate values.

Also in this case, the Hugging Face token you saved earlier will be automatically retrieved.

Note that, in this case, the dependencies `image` and `command` fields we discussed above are merged together and provided as values to the last argument `--submission.set-command`. Use the same order and change their values according to your specific use case.

#### Example 3: Using HF library

Instead of relying on `git` to download the model from HF, you can leverage the Hugging Face libraries, already provided by our pre-built dependencies images, to download the specific files you need directly from inside the agent python script.

To do so, you would need to:
- Create an agent script (e.g. `agent.py`) that contains the instructions to download the HF model, similar to the following example:
  ```python
  # Content of agent.py

  import os
  import argparse
  import diambra.arena

  from huggingface_hub import hf_hub_download, login as huggingface_hub_login

  def main(repo, cfg_file):

      # Retrieve the token from the ENV variables and login to HF
      if os.getenv("HF_TOKEN"):
          huggingface_hub_login(os.getenv("HF_TOKEN").strip())

      # Download the model weights and save the local path
      model_path = hf_hub_download(repo_id=repo, filename="./model_weights.zip")

      # Load the trained agent
      agent = PPO.load(model_path)

      # Environment settings setup and environment creation
      env = diambra.arena.make(game_id)

      # Agent-Environment loop
      obs, info = env.reset()

      while True:
          action, _ = agent.predict(obs, deterministic=False)

          obs, reward, terminated, truncated, info = env.step(action.tolist())

          if terminated or truncated:
              obs, info = env.reset()
              if info["env_done"] or test is True:
                  break

      # Close the environment
      env.close()

  if __name__ == "__main__":
      parser = argparse.ArgumentParser()
      parser.add_argument("--repo", type=str, required=True, help="Repository name")
      opt = parser.parse_args()

      main(opt.repo)
  ```
- Create a custom image based on one of our pre-built images with the appropriate dependencies, place the agent script inside it and push it to your docker image registry of choice
- Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image, create a file named `submission-manifest.yaml` with the following content:
  ```yaml
  mode: AIvsCOM
  image: docker-image-repo/docker-image-name:tag
  command:
    - python
    - "./agent.py"
    - "hf-repo-id"
  ```
  Changing `image` and `command` fields according to your specific use case.

  Note that you don't need to specify the `HF_TOKEN` `ENV` variable to be retrieved by your script as it will be automatically loaded by our CLI when using the `--submission.secrets-from=huggingface` option as shown below.

- Then, submit your agent using the manifest file:
  ```sh
  diambra agent submit --submission.secrets-from=huggingface --submission.manifest submission-manifest.yaml
  ```
  Also in this case, the Hugging Face token you saved earlier will be automatically retrieved.

#### Example 4: SheepRL - Using a Manifest File
This section shows how to submit a trained agent with SheepRL.

<a href="https://github.com/diambra/agents/blob/main/sheeprl/agent-ppo.py" target="_blank">Here</a> you can find an example of what the evaluation script should look like.
You need two files:
- A YAML configuration file, the one produced during training, that contains the configs of the agent and all the information needed to instantiate the environment.
- A `ckpt` file that contains the weights of the agent.

After retrieving these two files, you can load them with the script file into your huggingface repository.
An example of the files you need to retrieve is shown <a href="https://github.com/diambra/agents/blob/main/sheeprl/example-logs/runs/ppo/doapp/experiment" target="_blank">here</a>.

Assuming you are using the `arena-sheeprl-on3.10-bullseye` dependencies image and that you upload the files without subdirectories, create a file named `submission-manifest.yaml` with the following content:

```yaml
mode: AIvsCOM
image: diambra/arena-sheeprl-on3.10-bullseye:main
command:
  - python
  - "/sources/agent-ppo.py"
  - "--cfg_path"
  - "/sources/results/ppo/config.yaml"
  - "--checkpoint_path"
  - "/sources/results/ppo/ckpt_1024_0.ckpt"
sources:
  .: git+https://username:{{.Secrets.hf_token}}@huggingface.co/username/repository_name.git#ref=branch_name
```

{{% notice note %}}
<a href="https://huggingface.co/michele-milesi/diambra-agent-example/tree/main" target="_blank">Here</a> you can find the huggingface repository on which the example is based.
{{% /notice %}}

Replace `username` and `repository_name.git#ref=branch_name` with the appropriate values, and change `image` and `command` fields according to your specific use case.

Then, submit your agent using the manifest file:

```sh
diambra agent submit --submission.secrets-from=huggingface --submission.manifest submission-manifest.yaml
```

This will automatically retrieve the Hugging Face token you saved earlier.

### GitHub

These are the steps to submit your own agent hosted on GitHub:

1. Store your agent files (e.g. scripts and weights) in a private repository
2. Create your personal access token (<a href="https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic" target="_blank">official docs here</a>):
   - Go to "Settings" in the top-right corner of the GitHub website.
   - Click "Developer settings" at the bottom-left of the page.
   - Click "Personal access tokens", "Tokens (classic)" and then "Generate new token".
   - Give your token a name, select the necessary scopes (e.g., "repo" for accessing private repositories), and click "Generate token."
   - Copy the generated token and save it somewhere safe, as you won't be able to see it again.
3. Submit your AI agent:
   - Choose the appropriate dependencies docker image for your submission. We provide <a href="https://github.com/orgs/diambra/packages?repo_name=arena" target="_blank">different pre-built ones</a> giving access to various common third party libraries
   - Submit your agent as shown in the following examples

#### Example 1: Using a Manifest File (Recommended)

Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image, create a file named `submission-manifest.yaml` with the following content:

```yaml
mode: AIvsCOM
image: diambra/arena-stable-baselines3-on3.10-bullseye:main
command:
  - python
  - "/sources/agent.py"
  - "/sources/models/model.zip"
sources:
  .: git+https://username:{{.Secrets.token}}@github.com/username/repository_name.git#ref=branch_name
```

Replace `username` and `repository_name.git#ref=branch_name` with the appropriate values, and change `image` and `command` fields according to your specific use case.

Then, submit your agent using the manifest file:

```sh
diambra agent submit --submission.secret token=your_gh_token --submission.manifest submission-manifest.yaml
```

Replace `your_gh_token` with the GitHub token you saved earlier.

Note that this will clone your entire repository (including Git LFS files) and put its content inside the `/sources/` folder directly.

##### Specify Sources Explicitly

{{% notice warning %}}
Explicit sources specification will not work with Git LFS files, to submit them, the only option is to use the automatic `git clone` mechanism described above.
{{% /notice %}}

In case you don't want to clone all your repository, you can explicitly specify the source files you want to download as follows:

```yaml
---
mode: AIvsCOM
image: diambra/arena-stable-baselines3-on3.10-bullseye:main
command:
  - python
  - "/sources/agent.py"
  - "/sources/models/model.zip"
sources:
  agent.py: https://{{.Secrets.token}}@raw.githubusercontent.com/username/repo_name/path/to/trained-agent/your_agent.py
  models/model.zip: https://{{.Secrets.token}}@raw.githubusercontent.com/username/repo_name/path/to/nn-weights/your_model.zip
```

In case you have multiple source files you need to use, and you want to avoid to list them all, the best way to proceed is to leverage our automatic `git clone` feature described above. But if you really do not want to use it, you can add the source files to a zip archive, store it in your repository and leverage our automatic `unzip` feature as follows:

```yaml
mode: AIvsCOM
image: diambra/arena-stable-baselines3-on3.10-bullseye:main
command:
  - python
  - "/sources/data/agent.py"
  - "/sources/data/models/model.zip"
sources:
  data: https+unzip://{{.Secrets.token}}@raw.githubusercontent.com/username/repo_name/path/to/data/data.zip
```

Note that in the url of the zip file to be downloaded there is an additional `+unzip` string following `https`.

#### Example 2: Command Line Interface Only

If you want to avoid using submission files, you can use the command line to directly submit your agent. Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image:

```sh
diambra agent submit \
  --submission.mode AIvsCOM \
  --submission.source .=git+https://username:{{.Secrets.token}}@github.com/username/repository_name.git#ref=branch_name \
  --submission.secret token=your_gh_token \
  --submission.set-command \
  arena-stable-baselines3-on3.10-bullseye \
  python "/sources/agent.py" "/sources/models/model.zip"

```

Replace `username` and `repository_name.git#ref=branch_name` with the appropriate values and `your_gh_token` with the GitHub token you saved earlier.

Note that, in this case, the dependencies `image` and `command` fields we discussed above are merged together and provided as values to the last argument `--submission.set-command`. Use the same order and change their values according to your specific use case.