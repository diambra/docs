---
title: Custom Dependencies Image
weight: 60
math: true
---

Instead of using the pre-built dependencies docker images we provide, you may want/need to create custom ones. It can easily be done in just a few steps:

1. Create the Dockerfile containing the custom dependencies you need. Any mix of publicly available packages and repository, and copies of libraries you have in your local system work.

2. Build the docker image with your custom dependencies:

   ```shell
   docker build -t <registry>/<name>:<tag> .
   ```

   This will create the docker image and tag it. You can use any public registry, like <a href="https://quay.io" target="_blank">quay.io</a> or <a href="https://hub.docker.com" target="_blank">dockerhub,</a> but make sure the image is public.

3. Push the image to the registry:

    ```shell
    docker push <registry>/<name>:<tag>
    ```


{{% notice tip %}}
Examples of docker files to create dependencies images are on the <a href="https://github.com/diambra/arena/tree/main/images" target='_blank'>Arena repo,</a> where we automatically build images for the RL libraries we support.
{{% /notice %}}



Once these steps are completed, you can submit the agent to the platform using your custom dependencies images. Assuming you are in the very same situation explained in the examples shown in the <a href="/competitionplatform/howtosubmitanagent/submityourownagent/#example-1-using-a-manifest-file-suggested">Submit Your Own Agent</a> page, you would tweak them, respectively, as follows:

- Example 1: Command Line Interface Command

  Update the image name command line argument:
  ```sh
  diambra agent submit \
  --submission.mode AIvsCOM \
  --submission.source agent.py=https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/trained-agent/your_agent.py \
  --submission.source models/model.zip=https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/nn-weights/your_model.zip \
  --submission.secret token=your_gh_token \
  --submission.set-command \
  <registry>/<name>:<tag> \
  python "/sources/agent.py" "/sources/models/model.zip"
  ```

- Example 2: Using a Manifest File

  Update the image name in the submission manifest:
  ```yaml
  mode: AIvsCOM
  image: <registry>/<name>:<tag>
  command:
    - python
    - "/sources/agent.py"
    - "/sources/models/model.zip"
  sources:
    agent.py: https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/trained-agent/your_agent.py
    models/model.zip: https://{{.Secrets.token}}@raw.githubusercontent.com/path/to/nn-weights/your_model.zip
  ```

{{% notice warning %}}
Please note that the dependencies docker images needs to be public and will be publicly visible on the platform. Make sure you do not include in them any file you want to keep private.
{{% /notice %}}

{{% notice warning %}}
Currently, we can only process Docker images built for amd64 CPU architecture. So, if you are using MacOS with M1 or M2 CPUs, you need to explicitly tell Docker to do that at build time as follows:<br><br>1. Open Docker Desktop Dashboard / Preferences (cog icon) / Turn "Experimental Features" on & apply<br>2. Create a new builder instance with `docker buildx create --use`<br>3. Run `docker buildx build --platform linux/amd64 --push -t <image-tag> .`<br><br>Note that:<br>- If you can’t see an “Experimental Features” option, sign up for the <a href="https://www.docker.com/community/get-involved/developer-preview/" target="_blank">Docker developer program</a><br>- You have to push directly to a repository instead of doing it after build
{{% /notice %}}
