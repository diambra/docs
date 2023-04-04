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

{{% notice warning %}}
**Do not add your tokens directly in the submission YAML file, they will be publicly visible.**
{{% /notice %}}