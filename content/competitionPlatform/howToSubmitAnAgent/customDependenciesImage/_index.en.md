---
title: Custom Dependencies Image
weight: 60
math: true
---


{{% notice warning %}}
Currently, we can only process Docker images built for amd64 CPU architecture. So, if you are using MacOS with M1 or M2 CPUs, you need to explicitly tell Docker to do that at build time as follows:<br><br>1. Open Docker Desktop Dashboard / Preferences (cog icon) / Turn "Experimental Features" on & apply<br>2. Create a new builder instance with `docker buildx create --use`<br>3. Run `docker buildx build --platform linux/amd64 --push -t <image-tag> .`<br><br>Note that:<br>- If you can’t see an “Experimental Features” option, sign up for the <a href="https://www.docker.com/community/get-involved/developer-preview/" target="_blank">Docker developer program</a><br>- You have to push directly to a repository instead of doing it after build
{{% /notice %}}


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