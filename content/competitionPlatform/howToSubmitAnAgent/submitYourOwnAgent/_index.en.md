---
title: Submit Your Own Agent
weight: 50
math: true
---

These are the steps to submit your own agent:

1. Store your agent files (e.g. scripts and weights) in private repository, we will use GitHub as example
2. Create your personal access token (<a href="https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-personal-access-token-classic" target="_blank">official docs here</a>):
   - Go to "Settings" in the top-right corner of the GitHub website.
   - Click "Developer settings" at the bottom-left of the page.
   - Click "Personal access tokens" and then "Generate new token."
   - Give your token a name, select the necessary scopes (e.g., "repo" for accessing private repositories), and click "Generate token."
   - Copy the generated token and save it somewhere safe, as you won't be able to see it again.
3. Submit your AI agent:
   - Choose the appropriate dependencies docker image for your submission. We provide <a href="https://github.com/orgs/diambra/packages?repo_name=arena" target="_blank">different pre-built ones</a> giving access to various common third party libraries
   - Submit your agent as shown in the following examples

{{% notice tip %}}
To favor an easy start, we provide example agents files (scripts and weights) that work out-of-the-box (but are only minimally trained) in our <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents</a> repository, for both <a href="https://github.com/diambra/agents/tree/main/stable_baselines3" target="_blank">Stable Baselines 3</a> and <a href="https://github.com/diambra/agents/tree/main/ray_rllib" target="_blank">Ray RLlib.</a>
{{% /notice %}}

{{% notice warning %}}
**Do not add your tokens directly in the submission YAML file, they will be publicly visible.**
{{% /notice %}}

#### Example 1: Command Line Interface Command

Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image and have your agent's files stored on GitHub:

```sh
diambra agent submit \
  --submission.mode AIvsCOM \
  --submission.source agent.py=https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/trained-agent/your_agent.py \
  --submission.source models/model.zip=https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/nn-weights/your_model.zip \
  --submission.secret token=your_gh_token \
  --submission.set-command \
  arena-stable-baselines3-on3.10-bullseye \
  python "/sources/agent.py" "/sources/models/model.zip"

```

Replace `your_gh_token`, `your_agent.py` and `your_model.zip` with the appropriate values.

The `--submission.source` flag takes `URL<->path/in/container` mappings to download files to the specified path. The `{{ .Secrets.<something> }}` can be used to include secrets specified in the `--submission.secret` flag. Combining both flags, you can create a submission that includes secrets and sources to download your weights from your private repository.

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

#### Example 3: Adding Multiple Sources Easily

In case you have multiple source files you need to use, and you want to avoid to list them all, you have two options:
 - Clone the whole repository directly and leverage our automatic `git clone` feature
 - Add the source files to a zip archive, store it in your repository and leverage our automatic `unzip` feature

the two examples that follow show how the submission manifest is modified to use these features.

##### Clone a Sources Repository

Note that the url of the git repository to be cloned is prefixed with an additional `git+` string.

```yaml
---
mode: AIvsCOM
image: diambra/arena-stable-baselines3-on3.10-bullseye:main
command:
  - python
  - "/sources/agent.py"
  - "/sources/models/model.zip"
sources:
  .: git+https://{{.Secrets.token}}@github.com/path/to/your_repo.git#ref=branch_name
```

##### Automatically Unzip Sources

Note that in the url of the zip file to be downloaded there is an additional `+unzip` string following `https`.

```yaml
mode: AIvsCOM
image: diambra/arena-stable-baselines3-on3.10-bullseye:main
command:
  - python
  - "/sources/data/agent.py"
  - "/sources/data/models/model.zip"
sources:
  data: https+unzip://{{.Secrets.token}}@raw.githubusercontent.com/path/to/data/data.zip
```