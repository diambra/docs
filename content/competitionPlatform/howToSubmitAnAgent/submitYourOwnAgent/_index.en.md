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
   - Click "Personal access tokens", "Tokens (classic)" and then "Generate new token".
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

#### Example 1: Using a Manifest File (Suggested)

Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image, and have your agent's files stored on GitHub, create a file named `submission-manifest.yaml` with the following content:

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
diambra agent submit  --submission.secret token=your_gh_token --submission.manifest submission-manifest.yaml
```

Replace `your_gh_token` with the GitHub token you saved earlier.

Note that this will clone your entire repository (including Git LFS files) and put its content inside the `/sources/` folder directly.

##### Specify Sources Explicitly

{{% notice warning %}}
Explicit sources specification will now work with Git LFS files, to submit them, the only option is to use the automatic `git clone` mechanism described above.
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

If you want to avoid using submission files, you can use the command line to directly submit your agent. Assuming you are using the `arena-stable-baselines3-on3.10-bullseye` dependencies image and have your agent's files stored on GitHub:

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