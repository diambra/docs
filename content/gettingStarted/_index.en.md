---
title: Getting Started
weight: 20
---

### Index

<div style="font-size:1.125rem;">

- <a href="./#prerequisites">Prerequisites</a>
- <a href="./#basic-usage">Basic Usage</a>
    - <a href="./#launcher-options">Launcher Options</a>
    - <a href="./#environment-run">Environment Run</a>
        - <a href="./#random-agent-in-dead-or-alive-headless-mode">Random Agent in Dead Or Alive++ (Headless Mode)</a>
        - <a href="./#random-agent-in-dead-or-alive-with-gui-support">Random Agent in Dead Or Alive++ (with GUI Support)</a>
        - <a href="./#gpu-docker-image-linux-only">Bash Terminal With Python Packages Persistent Install</a>
- <a href="./#advanced-usage">Advanced Usage</a>
    - <a href="./#gpu-docker-image-linux-only">GPU Docker Image (Linux Only)</a>
        - <a href="./#check-gpu-availability-with-tensorflow">Check GPU Availability with TensorFlow</a>
    - <a href="./#docker-commands-details">Docker Commands Details</a>
        - <a href="./#headless-execution">Headless Execution</a>
        - <a href="./#gui-supported-execution">GUI-Supported Execution</a>

</div>

### Prerequisites

- Installation completed and successfully tested as described in <a href="/installation/">Installation</a> section
- <a href="/installation/">TODO: UPDATE LINK Examples</a> provided with DIAMBRA Arena repo downloaded
- ROMs downloaded and all placed in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`

{{% notice tip %}}
To avoid specifying ROMs for every command you run, you can define a specific environment variable (See <a href="/installation/">Installation</a> section for details).
{{% /notice %}}

### Basic Usage

In what follows the most concise script example available will be used, `diambraArenaGist.py`, featuring a random agent playing Dead Or Alive ++. It has been selected to keep things as simple as possible, but every python script can be used in the very same way.

The complete script consist of just a few lines and is reported below:
```python {linenos=inline}
 import diambraArena 

 # Mandatory environment settings
 settings = {}
 settings["gameId"] = "doapp"
 settings["romsPath"] = "/path/to/roms/"
 
 # Environment creation
 env = diambraArena.make("MyEnv", settings)
 
 # Environment reset
 observation = env.reset()
 
 # Agent-Environment interaction loop
 while True:

     # Action sampling
     actions = env.action_space.sample()
 
     # Environment stepping
     observation, reward, done, info = env.step(actions)
 
     # Episode end (Done condition) check
     if done:
         # Environment reset after episode's end
         observation = env.reset()
         # Interrupting interaction loop at episode's end
         break
 
 # Environment close
 env.close()
```

{{% notice note %}}                                                             
More complex and complete examples can be found in the <a href="./examples/">Examples</a> section.
{{% /notice %}}           

#### Launcher Options (Docker Only)

To allow a very smooth execution, we included a *launcher script* in DIAMBRA Arena repo, inside the <a href="/installation/">TODO: UPDATE LINK Examples</a> folder. It is a bash/batch script named `diambraArena.sh`/`diambraArena.bat` for Linux-MacOS/Windows respectively. 

It provides a simple interface to easily perform many useful tasks, like:
 - Validating a ROM file
 - Executing a local python script, with and without GUI support (X-server)
 - Executing a terminal command inside the Docker container
 - Opening a live terminal inside the Docker container with optional support for persistent update of the container itself
 - Running the CPU only or the GPU container version (Linux Only)

The next block shows how to print out all available options for every OS.

{{< tabs groupId="noLinuxSource">}}
{{% tab name="Linux Docker / MacOS" %}}

```shell
./diambraArena.sh -h
```

Output:

```shell
Usage:

  ./diambraArena.sh [OPTIONS]
 
OPTIONS:
  -h Prints out this help message.
 
  -l Prints out available games list.
 
  -t <romFile.zip> Checks ROM file validity.
 
  -r "<path>" Specify your local path to where game roms are located.
              (Mandatory to run environments and to check ROM validity.)
 
  -s <file> Python script to run inside docker container.
            Must be located in the folder from where the bash script is executed.
 
  -c <command> Command to be executed at docker container startup.
               Can be used to start and interactive linux shell, for example to install pip packages.
 
  -g <X> Specify if to run in Headless mode (X=0, default) or with GUI support (X=1)
 
  -d <DEVICE> Specify if to run CPU docker image (DEVICE=CPU, default)
              or the one with NVIDIA GPU Support (DEVICE=GPU)
              Requires Nvidia-Docker Toolkit installed
 
  -v <name> Specify the name of the volume where to store pip packages
            installed inside the container to make them persistent. (Optional)
 
Examples:
  - Availble games list with details: ./diambraArena.sh -l
 
  - ROM File Validation: ./diambraArena.sh -r "your/roms/local/path"
                                           -t romFile.zip
 
  - Headless (CPU): ./diambraArena.sh -r "your/roms/local/path"
                                      -s yourPythonScriptInCurrentDir.py
                                      -v yourVolumeName (optional)
 
  - Headless (GPU): ./diambraArena.sh -r "your/roms/local/path"
                                      -s yourPythonScriptInCurrentDir.py
                                      -d GPU
                                      -v yourVolumeName (optional)
 
  - With GUI (CPU): ./diambraArena.sh -r "your/roms/local/path"
                                      -s yourPythonScriptInCurrentDir.py
                                      -g 1
                                      -v yourVolumeName (optional)
 
  - Terminal (CPU): ./diambraArena.sh -c bash
                                      -v yourVolumeName (optional)
 
  - CUDA Installation Test (GPU): ./diambraArena.sh -c "cat /proc/driver/nvidia/version; nvcc -V"
                                                    -d GPU
```

{{% /tab %}}
{{% tab name="Win" %}}

```shell
diambraArena.bat -h
```

Output:

```shell
Usage:

  diambraArena.bat [OPTIONS]

OPTIONS:
  -h Prints out this help message.

  -l Prints out available games list.

  "ROMCHECK=<romFile>.zip" Check ROM file validity.

  "ROMSPATH=<path>" Specify your local path where game ROMs are located. 
                    (Mandatory to run environments.)

  "PYTHONFILE=<file>" Python script to run inside docker container. 
                      Must be located in the folder from where the batch script is executed.

  "CMDTOEXEC=<command>" Command to be executed at docker container startup.
                        Can be used to start and interactive linux shell, for example to install pip packages.

  "GUI=<X>" Specify if to run in Headless mode (X=0, default) or with GUI support (X=1)

  "ENVDISPLAYIP=<vEthernetIP>" Specify the vEthernet IP Address on which the Virtual X Server is listening. 
                               The address can be retrieved using `ipconfig` command, 
                               look for `Default Switch` or `WSL` in connection details.
                               (Optional, the script will try to recover it automatically.)
 
  "XSRVPATH=<path>" Specify where Windows X Server executable is located.
                    Standard location is usually `C:\Program Files\vcxsrv.exe`
                    (Optional, the script will try to recover it automatically.)
 
  "VOLUME=<name>" Specify the name of the volume where to store pip packages 
                  installed inside the container to make them persistent. (Optional)

Examples:
  - Availble games list with details: diambraArena.bat -l

  - ROM File Validation: diambraArena.bat "ROMSPATH=your\roms\local\path"
                                          "ROMCHECK=<romFile>.zip"

  - Headless: diambraArena.bat "ROMSPATH=your\roms\local\path"
                               "PYTHONFILE=yourPythonScriptInCurrentDir.py"
                               "VOLUME=yourVolumeName" (optional)

  - With GUI: diambraArena.bat "ROMSPATH=your\roms\local\path" 
                               "PYTHONFILE=yourPythonScriptInCurrentDir.py"
                               "GUI=1" 
                               "ENVDISPLAYIP=<vEthernetIP>" (optional)
                               "XSRVPATH=<pathToVcXsrv>" (optional)
                               "VOLUME=yourVolumeName" (optional)

  - Terminal: diambraArena.bat "CMDTOEXEC=bash"
                               "VOLUME=yourVolumrName" (optional)
```

{{% /tab %}}
{{< /tabs >}}

#### Environment Run

The next three code blocks show the three most important use-cases covering the vast majority of typical interaction needed:
- Running the environment through a python script with GUI support
- Running the environment through a python script in Headless mode
- Opening a terminal inside the Docker container using volumes to make operations, like Python packages install, persistent

A typical interaction when using DIAMBRA Arena for Reinforcement Learning research and experimentation could be, for example:

1. Install all dependencies needed to train your Deep RL agent inside the container and make them persistent (i.e. installing Stable-Baselines from the terminal)
2. Prepare your Python training script on your local machine and launch it in headless mode
3. Perform evaluation of your trained agent taking a look at how it behaves running your local Python evaluation script inside the Docker with GUI Support

{{% notice note %}}                                                             
The entire folder where the launcher script is located will be loaded inside the container. The Python script launched needs to be in the same folder of the launcher script.
{{% /notice %}} 

##### Random Agent in Dead Or Alive++ (Headless Mode)

{{< tabs groupId="linuxSource">}}
{{% tab name="Linux Docker / MacOS" %}}

```shell
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py
```

{{% /tab %}}
{{% tab name="Win" %}}

```shell
diambraArena.bat "ROMSPATH=your/roms/local/path" "PYTHONFILE=diambraArenaGist.py"
```

{{% /tab %}}
{{% tab name="Linux Source" %}}

Add 

```python
diambraKwargs["headless"] = True
```

to environment keyword arguments in `diambraArenaGist.py` script

```shell
python diambraArenaGist.py --romsPath "your/roms/local/path"
```

{{% /tab %}}
{{< /tabs >}}

##### Random Agent in Dead Or Alive++ (with GUI Support)

{{< tabs groupId="linuxSource">}}
{{% tab name="Linux Docker / MacOS" %}}

{{% notice note %}}
Running environments with GUI support requires the Docker container to connect to a virtual X server on the host machine. The launch script tries to establish this connection automatically, but it may require additional configuration. If you are not able to run the next code block, make sure you followed the installation instructions <a href="/installation/#prerequisites">here</a> (MacOS) and ask for support on our <a href="https://discord.gg/tFDS2UN5sv" target="_blank">Discord Server</a>. 
{{% /notice %}}

```shell
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py -g 1
```

{{% /tab %}}
{{% tab name="Win" %}}

{{% notice note %}}
Running environments with GUI support requires the Docker container to connect to a virtual X server on the host machine. The launch script tries to establish this connection automatically, but it may require additional configuration. If you are not able to run the next code block, make sure you followed the installation instructions <a href="/installation/#prerequisites">here</a> and ask for support on our <a href="https://discord.gg/tFDS2UN5sv" target="_blank">Discord Server</a>. 
{{% /notice %}}

```shell
diambraArena.bat "ROMSPATH=your/roms/local/path" "PYTHONFILE=diambraArenaGist.py" "GUI=1"
```

{{% notice info %}}
Increasing game window size could cause relevant slowdowns to the environments. GUI-supported executions are mainly thought for evaluation purposes. Make sure to use headless mode / deactivate rendering for maximum environment speed (e.g. during training).
{{% /notice %}}
{{% /tab %}}
{{% tab name="Linux Source" %}}

```shell
python diambraArenaGist.py --romsPath "your/roms/local/path"
```

{{% /tab %}}
{{< /tabs >}}

##### Bash Terminal With Python Packages Persistent Install

In order to make Python packages installation persistent inside the Docker container, Docker volumes are used. The container's Python package folder is linked to a folder in user's local filesystem (named `yourVolumeName`) where all modifications are saved.

{{< tabs groupId="noLinuxSource">}}
{{% tab name="Linux Docker / MacOS" %}}

```shell
./diambraArena.sh -c bash -v yourVolumeName
```

{{% /tab %}}
{{% tab name="Win" %}}

```shell
diambraArena.bat "CMDTOEXEC=bash" "VOLUME=yourVolumeName"
```

{{% /tab %}}
{{< /tabs >}}

### Advanced usage

#### GPU Docker Image (Linux Only)

{{% notice info %}}
GPU access from docker images is natively supported by Linux hosts only, provided <a href="https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html" target="_blank">Nvidia Container Toolkit</a> has been installed. Windows host systems can support it only via experimental features (for now), as described <a href="https://docs.nvidia.com/cuda/wsl-user-guide/index.html" target="_blank">here</a> and <a href="https://docs.docker.com/desktop/windows/wsl/#gpu-support" target="_blank">here</a>. <span style="color:#333333; font-weight: bolder;">It is highly recommended to rely on Linux hosts to leverage this feature.</span>
{{% /notice %}}

##### Check GPU Availability with TensorFlow

The next code block verifies if the GPU Docker image has access to the machine GPU by sending a command to the terminal that: first, installs Tensorflow with PIP, and then executes a short Python script that runs the TF check for GPU availability.

```shell
./diambraArena.sh -d GPU -c "pip install tensorflow-gpu==1.14; python -c \"import tensorflow as tf; print('GPU available =', tf.test.is_gpu_available())\""
```

A successful test output would look like the following:

```shell
2022-01-11 00:55:20.934284: I tensorflow/core/platform/cpu_feature_guard.cc:142] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
2022-01-11 00:55:20.993143: I tensorflow/stream_executor/platform/default/dso_loader.cc:42] Successfully opened dynamic library libcuda.so.1
2022-01-11 00:55:21.118090: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:1005] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2022-01-11 00:55:21.119985: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x558e190bb810 executing computations on platform CUDA. Devices:
2022-01-11 00:55:21.120147: I tensorflow/compiler/xla/service/service.cc:175]   StreamExecutor device (0): GeForce MX130, Compute Capability 5.0
2022-01-11 00:55:21.156851: I tensorflow/core/platform/profile_utils/cpu_utils.cc:94] CPU Frequency: 1999965000 Hz
2022-01-11 00:55:21.157454: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x558e194a2d50 executing computations on platform Host. Devices:
2022-01-11 00:55:21.157471: I tensorflow/compiler/xla/service/service.cc:175]   StreamExecutor device (0): <undefined>, <undefined>
2022-01-11 00:55:21.158922: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:1005] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2022-01-11 00:55:21.159225: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1640] Found device 0 with properties: 
name: GeForce MX130 major: 5 minor: 0 memoryClockRate(GHz): 1.189
pciBusID: 0000:02:00.0
2022-01-11 00:55:21.161548: I tensorflow/stream_executor/platform/default/dso_loader.cc:42] Successfully opened dynamic library libcudart.so.10.0
2022-01-11 00:55:21.190554: I tensorflow/stream_executor/platform/default/dso_loader.cc:42] Successfully opened dynamic library libcublas.so.10.0
2022-01-11 00:55:21.207398: I tensorflow/stream_executor/platform/default/dso_loader.cc:42] Successfully opened dynamic library libcufft.so.10.0
2022-01-11 00:55:21.213738: I tensorflow/stream_executor/platform/default/dso_loader.cc:42] Successfully opened dynamic library libcurand.so.10.0
2022-01-11 00:55:21.255892: I tensorflow/stream_executor/platform/default/dso_loader.cc:42] Successfully opened dynamic library libcusolver.so.10.0
2022-01-11 00:55:21.291881: I tensorflow/stream_executor/platform/default/dso_loader.cc:42] Successfully opened dynamic library libcusparse.so.10.0
2022-01-11 00:55:21.413018: I tensorflow/stream_executor/platform/default/dso_loader.cc:42] Successfully opened dynamic library libcudnn.so.7
2022-01-11 00:55:21.413770: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:1005] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2022-01-11 00:55:21.414793: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:1005] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2022-01-11 00:55:21.416114: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1763] Adding visible gpu devices: 0
2022-01-11 00:55:21.416795: I tensorflow/stream_executor/platform/default/dso_loader.cc:42] Successfully opened dynamic library libcudart.so.10.0
2022-01-11 00:55:21.420757: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1181] Device interconnect StreamExecutor with strength 1 edge matrix:
2022-01-11 00:55:21.420831: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1187]      0 
2022-01-11 00:55:21.421236: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1200] 0:   N 
2022-01-11 00:55:21.422479: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:1005] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2022-01-11 00:55:21.423731: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:1005] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2022-01-11 00:55:21.425889: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1326] Created TensorFlow device (/device:GPU:0 with 1192 MB memory) -> physical GPU (device: 0, name: GeForce MX130, pci bus id: 0000:02:00.0, compute capability: 5.0)
True
```

#### Docker Commands Details

The next code blocks shows how the launcher script handles the execution. This can be useful to understand in case one wants to tweak it to accommodate different/additional requirements/needs. The code is found at the bottom of the launcher scripts.

##### Headless Execution

{{< tabs groupId="gpuGpu">}}
{{% tab name="Linux/MacOS CPU" %}}

```bash
volume="-v $volumeName:/usr/local/lib/python3.6/dist-packages/"
romsPath="--mount src=$romsPath,target="/opt/diambraArena/roms",type=bind"

docker run -it --rm --privileged $volume $romsPath \              
 --mount src=$(pwd),target="/opt/diambraArena/code",type=bind \             
 -v diambraService:/root/ --name diambraArena diambra/diambra-arena:base \
  sh -c "cd /opt/diambraArena/code/ && $cmd" 
```

Where `volumeName`, `romsPath` and `cmd` are are built by the launcher using arguments passed to the script by the user, and the launch command is composed by the following parts:

- `docker run -it --rm --privileged`: running the docker image in interactive mode (`-it`), removing the container once exited (`--rm`) and granting a given level of permissions (`--privileged`)
- `-v $volumeName:/usr/local/lib/python3.6/dist-packages/`: creating/linking a volume named `volumeName` in user's local workspace with container's default Python packages installation directory to make changes persistent (they will be saved inside the volume)
- `--mount src=$romsPath,target="/opt/diambraArena/roms",type=bind`: mounting and binding the user's local ROMs folder to a specific location inside the container where ROMs are expected to be located (`/opt/diambraArena/roms`)
- `--mount src=$(pwd),target="/opt/diambraArena/code",type=bind`: mounting and binding the user's local folder from where the launcher script is executed, to a specific location inside the container where code to run is expected to be located (`/opt/diambraArena/code`)
- `-v diambraService:/root/`: creating/linking a volume to container's home directory for DIAMBRA internal services
- `--name diambraArena diambra/diambra-arena:base`: specifying the name of the container to be created (`--name diambraArena`) and the image to load (`diambra/diambra-arena:base`)
- `sh -c "cd /opt/diambraArena/code/ && $cmd`: specifying the command to be executed in container's shell

{{% /tab %}}
{{% tab name="Linux GPU" %}}

```bash
volume="-v $volumeName:/usr/local/lib/python3.6/dist-packages/"
romsPath="--mount src=$romsPath,target="/opt/diambraArena/roms",type=bind"

docker run -it --rm --gpus all --privileged $volume $romsPath \              
 --mount src=$(pwd),target="/opt/diambraArena/code",type=bind \             
 -v diambraService:/root/ --name diambraArena diambra/diambra-arena:gpu-cuda10.0 \
  sh -c "cd /opt/diambraArena/code/ && $cmd" 
```

Where `volumeName`, `romsPath` and `cmd` are are built by the launcher using arguments passed to the script by the user, and the launch command is composed by the following parts:

- `docker run -it --rm --gpus all --privileged`: running the docker image in interactive mode (`-it`), removing the container once exited (`--rm`), making all machine's gpus accessible to the container (`--gpus all`), and granting a given level of permissions (`--privileged`)
- `-v $volumeName:/usr/local/lib/python3.6/dist-packages/`: creating/linking a volume named `volumeName` in user's local workspace with container's default Python packages installation directory to make changes persistent (they will be saved inside the volume)
- `--mount src=$romsPath,target="/opt/diambraArena/roms",type=bind`: mounting and binding the user's local ROMs folder to a specific location inside the container where ROMs are expected to be located (`/opt/diambraArena/roms`)
- `--mount src=$(pwd),target="/opt/diambraArena/code",type=bind`: mounting and binding the user's local folder from where the launcher script is executed, to a specific location inside the container where code to run is expected to be located (`/opt/diambraArena/code`)
- `-v diambraService:/root/`: creating/linking a volume to container's home directory for DIAMBRA internal services
- `--name diambraArena diambra/diambra-arena:gpu-cuda10.0`: specifying the name of the container to be created (`--name diambraArena`) and the image to load (`diambra/diambra-arena:gpu-cuda10.0`)
- `sh -c "cd /opt/diambraArena/code/ && $cmd`: specifying the command to be executed in container's shell

{{% /tab %}}
{{% tab name="Win CPU" %}}

```bat
  set "CURDIR="%cd%"" 
  set VOLUME=-v %VOLUME%:/usr/local/lib/python3.6/dist-packages/ 
  set ROMSPATH=--mount src=%ROMSPATH%,target="/opt/diambraArena/roms",type=bind

  docker run -it --rm --privileged %ROMSPATH% %VOLUME% ^                        
   --mount src=%CURDIR%,target="/opt/diambraArena/code",type=bind ^             
   -v diambraService:/root/ --name diambraArena diambra/diambra-arena:base ^                             
    sh -c "cd /opt/diambraArena/code/ && !CMDTOEXEC!" 
```

Where `VOLUME`, `ROMSPATH` and `CMDTOEXEC` are are built by the launcher using arguments passed to the script by the user, and the launch command is composed by the following parts:

- `docker run -it --rm --privileged`: running the docker image in interactive mode (`-it`), removing the container once exited (`--rm`), and granting a given level of permissions (`--privileged`)
- `-v %VOLUME%:/usr/local/lib/python3.6/dist-packages/`: creating/linking a volume named `VOLUME` in user's local workspace with container's default Python packages installation directory to make changes persistent (they will be saved inside the volume)
- `--mount src=%ROMSPATH%,target="/opt/diambraArena/roms",type=bind`: mounting and binding the user's local ROMs folder to a specific location inside the container where ROMs are expected to be located (`/opt/diambraArena/roms`)
- `--mount src=%CURDIR%,target="/opt/diambraArena/code",type=bind`: mounting and binding the user's local folder from where the launcher script is executed, to a specific location inside the container where code to execute is expected to be located (`/opt/diambraArena/code`)
- `-v diambraService:/root/`: creating/linking a volume to container's home directory for DIAMBRA internal services
- `--name diambraArena diambra/diambra-arena:base`: specifying the name of the container to be created (`--name diambraArena`) and the image to load (`diambra/diambra-arena:base`)
- `sh -c "cd /opt/diambraArena/code/ && !CMDTOEXEC!`: specifying the command to be executed in container's shell

{{% /tab %}}
{{< /tabs >}}

##### GUI-Supported Execution

{{< tabs groupId="guiExec">}}
{{% tab name="Linux" %}}

```bash
volume="-v $volumeName:/usr/local/lib/python3.6/dist-packages/"
romsPath="--mount src=$romsPath,target="/opt/diambraArena/roms",type=bind"

./x11docker --cap-default --hostipc --network=host --name=diambraArena --wm=host \
 --pulseaudio --size=1024x600 -- --privileged $volume $romsPath \ 
 --mount src=$(pwd),target="/opt/diambraArena/code",type=bind \             
 -v diambraService:/root/ -- diambra/diambra-arena:base &>/dev/null & sleep 4s; \                                    
  docker exec -u 0 --privileged -it diambraArena \                          
  sh -c "set -m; cd /opt/diambraArena/code/ && $cmd"; pkill -f "bash ./x11docker*"
```

Where `volumeName`, `romsPath` and `cmd` are are built by the launcher using arguments passed to the script by the user, and the launch command is composed by the following parts:

- `./x11docker --cap-default --hostipc --network=host --name=diambraArena ` `--wm=host \
 --pulseaudio --size=1024x600 -- --privileged`: running the application providing x11 support for Docker on Linux `x11docker` (credits to the <a href="https://github.com/mviereck/x11docker" target="_blank">x11docker team!</a>) included in the examples folder provided by DIAMBRA Arena Repo using its default options for networking (`--cap-default --hostipc --network=host --wm=host`), specifying the container name (`--name=diambraArena`), specifying support for audio (`--pulseaudio`), setting viewport resolution (`--size=1024x600`), and granting a given level of permissions (`--privileged`)
- `-v $volumeName:/usr/local/lib/python3.6/dist-packages/`: creating/linking a volume named `volumeName` in user's local workspace with container's default Python packages installation directory to make changes persistent (they will be saved inside the volume)
- `--mount src=$romsPath,target="/opt/diambraArena/roms",type=bind`: mounting and binding the user's local ROMs folder to a specific location inside the container where ROMs are expected to be located (`/opt/diambraArena/roms`)
- `--mount src=$(pwd),target="/opt/diambraArena/code",type=bind`: mounting and binding the user's local folder from where the launcher script is executed, to a specific location inside the container where code to run is expected to be located (`/opt/diambraArena/code`)
- `-v diambraService:/root/`: creating/linking a volume to container's home directory for DIAMBRA internal services
- `diambra/diambra-arena:base &>/dev/null & sleep 4s;`: specifying the image to load (`diambra/diambra-arena:base`), forwarding the output to `/dev/null` to keep the execution alive (`&>/dev/null`), putting the process in the background and waiting for 4 seconds before moving to the next command (`& sleep 4s;`)
- `docker exec -u 0 --privileged -it diambraArena`: accessing the running container specifying the user (`-u 0`), granting a given level of permissions (`--privileged`), in interactive mode (`-it`), and identifying it by its name (`diambraArena`)
- `sh -c "set -m; cd /opt/diambraArena/code/ && $cmd`: specifying the command to be executed in container's shell
- `pkill -f "bash ./x11docker*`: killing the `x11docker` application running in background once the container execution ends 

{{% /tab %}}
{{% tab name="Win" %}}

```bat
  set "CURDIR="%cd%"" 
  set VOLUME=-v %VOLUME%:/usr/local/lib/python3.6/dist-packages/ 
  set ROMSPATH=--mount src=%ROMSPATH%,target="/opt/diambraArena/roms",type=bind

  START /B CMD /C CALL "!XSRVPATH!" -noprimary -nowgl -ac -displayfd 664 -screen 0 400x300@1
  docker run -it --rm --privileged -e DISPLAY="!ENVDISPLAYIP!:0.0" %ROMSPATH% %VOLUME% ^
   --mount src=%CURDIR%,target="/opt/diambraArena/code",type=bind ^             
   -v diambraService:/root/ --name diambraArena diambra/diambra-arena:base ^                             
    sh -c "cd /opt/diambraArena/code/ && %CMDTOEXEC%"                            
  TASKKILL /IM vcxsrv.exe /F
```

Where `VOLUME`, `ROMSPATH`, `ENVDISPLAYIP`, `XSRVPATH` and `CMDTOEXEC` are are built by the launcher using arguments passed to the script by the user, and the launch command is composed by the following parts:

- `START /B CMD /C CALL "!XSRVPATH!" -noprimary -nowgl -ac -displayfd 664 -screen 0 400x300@1`: running in background (`/B`) the `vcxsrv.exe` virtual X server with all its standard options, specifying in particular the viewport resolution (`-screen 0 400x300@1`)
- `docker run -it --rm --privileged -e DISPLAY="!ENVDISPLAYIP!:0.0"`: running the docker image in interactive mode (`-it`), removing the container once exited (`--rm`), granting a given level of permissions (`--privileged`), and setting the environment variable for the remote display (` -e DISPLAY="!ENVDISPLAYIP!:0.0"`)
- `-v %VOLUME%:/usr/local/lib/python3.6/dist-packages/`: creating/linking a volume named `VOLUME` in user's local workspace with container's default Python packages installation directory to make changes persistent (they will be saved inside the volume)
- `--mount src=%ROMSPATH%,target="/opt/diambraArena/roms",type=bind`: mounting and binding the user's local ROMs folder to a specific location inside the container where ROMs are expected to be located (`/opt/diambraArena/roms`)
- `--mount src=%CURDIR%,target="/opt/diambraArena/code",type=bind`: mounting and binding the user's local folder from where the launcher script is executed, to a specific location inside the container where code to execute is expected to be located (`/opt/diambraArena/code`)
- `-v diambraService:/root/`: creating/linking a volume to container's home directory for DIAMBRA internal services
- `--name diambraArena diambra/diambra-arena:base`: specifying the name of the container to be created (`--name diambraArena`) and the image to load (`diambra/diambra-arena:base`)
- `sh -c "cd /opt/diambraArena/code/ && !CMDTOEXEC!`: specifying the command to be executed in container's shell
- `TASKKILL /IM vcxsrv.exe /F`: killing the `vcxsrv.exe` application running in background once the container execution ends 

{{% /tab %}}
{{% tab name="MacOS" %}}

```bash                                                                         
volume="-v $volumeName:/usr/local/lib/python3.6/dist-packages/"                 
romsPath="--mount src=$romsPath,target="/opt/diambraArena/roms",type=bind" 

socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\" &>/dev/null & sleep 15s; open -a xquartz; sleep 5s; \
docker run -it --rm --privileged -e DISPLAY="$envDisplayIp:0.0" $volume $romsPath \
 --mount src=$(pwd),target="/opt/diambraArena/code",type=bind \             
 -v diambraService:/root/ --name diambraArena diambra/diambra-arena:base \                                           
  sh -c "cd /opt/diambraArena/code/ && $cmd"
```

Where `volumeName`, `romsPath`, `DISPLAY`, `envDisplayIp` and `cmd` are are built by the launcher using arguments passed to the script by the user, and the launch command is composed by the following parts:

- `socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\" &>/dev/null & sleep 15s;`: running `socat` to bridge communication between Docker container and `xquartz` virtual X server, forwarding the output to `/dev/null` to keep the process alive (`&>/dev/null`), putting the process in the background and waiting for 15 seconds before moving to the next command (`& sleep 15s;`) 
- `open -a xquartz; sleep 5s;`: opening xquartz x display server to receive the container stream and pausing script execution for 5s (`sleep 5s`)
- `docker run -it --rm --privileged -e DISPLAY="$envDisplayIp:0.0"`: running the docker image in interactive mode (`-it`), removing the container once exited (`--rm`), granting a given level of permissions (`--privileged`), and setting the environment variable for the remote display (`-e DISPLAY="$envDisplayIp:0.0"`)
- `-v $volumeName:/usr/local/lib/python3.6/dist-packages/`: creating/linking a volume named `volumeName` in user's local workspace with container's default Python packages installation directory to make changes persistent (they will be saved inside the volume)
- `--mount src=$romsPath,target="/opt/diambraArena/roms",type=bind`: mounting and binding the user's local ROMs folder to a specific location inside the container where ROMs are expected to be located (`/opt/diambraArena/roms`)
- `--mount src=$(pwd),target="/opt/diambraArena/code",type=bind`: mounting and binding the user's local folder from where the launcher script is executed, to a specific location inside the container where code to run is expected to be located (`/opt/diambraArena/code`)
- `-v diambraService:/root/`: creating/linking a volume to container's home directory for DIAMBRA internal services
- `--name diambraArena diambra/diambra-arena:base`: specifying the name of the container to be created (`--name diambraArena`) and the image to load (`diambra/diambra-arena:base`)
- `sh -c "cd /opt/diambraArena/code/ && $cmd`: specifying the command to be executed in container's shell

{{% /tab %}}
{{< /tabs >}}
