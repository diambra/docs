---
title: Getting Started
weight: 20
---

<div style="font-size:20px;">

### Index

- <a href="/gettingstarted/#prerequisites">Prerequisites</a>
- <a href="/gettingstarted/#basic-usage">Basic Usage</a>
    - <a href="/gettingstarted/#launcher-options">Launcher Options</a>
    - <a href="/gettingstarted/#environment-run">Environment Run</a>
        - <a href="/gettingstarted/#random-agent-gui">Random Agent in Dead Or Alive++ (with GUI Support)</a>
        - <a href="/gettingstarted/#random-agent-headless">Random Agent in Dead Or Alive++ (Headless Mode)</a>
        - <a href="/gettingstarted/#bash-terminal">Bash Terminal With Python Packages Persistent Install</a>
- <a href="/gettingstarted/#advanced-usage">Advanced Usage</a>
    - <a href="/gettingstarted/#gpu-docker-image-linux-only">GPU Docker Image (Linux Only)</a>
        - <a href="/gettingstarted/#gpu-availability">Check GPU Availability with TensorFlow</a>
    - <a href="/gettingstarted/#docker-commands-details">Docker Commands Details</a>
        - <a href="/gettingstarted/#headless-execution">Headless Execution</a>
        - <a href="/gettingstarted/#gui-supported-execution">GUI-Supported Execution</a>

### Prerequisites

- Installation completed and successfully tested as described in <a href="/installation/">Installation</a> section
- <a href="/installation/">TODO: UPDATE LINK Examples</a> provided with DIAMBRA Arena repo downloaded
- ROMs downloaded and all placed in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`

{{% notice tip %}}
<span style="font-size:20px;">To avoid specifying ROMs for every command you run, you can define a specific environment variable (See <a href="/installation/">Installation</a> section for details).</span>
{{% /notice %}}

### Basic Usage

In what follows we will always use the most concise script example available, `diambraArenaGist.py`, featuring a random agent playing Dead Or Alive ++. We selected it to keep things as simple as possible, but every python script can be used in the very same way.

{{% notice note %}}                                                             
<span style="font-size:20px;">More complex and complete examples can be found in the <a href="/gettingstarted/examples/">Examples</a> section.</span>
{{% /notice %}}           

#### Launcher Options (Docker only)

To allow a very smooth execution, we included a *launcher script* in DIAMBRA Arena repo, inside the <a href="/installation/">TODO: UPDATE LINK Examples</a> folder. It is a bash/batch script named `diambraArena.sh`/`diambraArena.bat` for Linux-MacOS/Windows respectively. 

It provides a simple interface to easily perform many useful tasks, like:
 - Validating a ROM file
 - Executing a local python script, with and without GUI support (X-server)
 - Executing a terminal command inside the Docker container
 - Opening a live terminal inside the Docker container with optional support for persistent update of the container itself
 - Running the CPU only or the GPU container version (Linux Only)

The next block shows how to print out all available options for every OS.

</div>

{{< tabs >}}
{{% tab name="Linux (Docker) / MacOS" %}}
```bash
./diambraArena.sh -h
```
<span style="font-size:20px;">Output:</span>
```terminal
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
```batch
diambraArena.bat -h
```
<span style="font-size:20px;">Output:</span>

```terminal
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

<div style="font-size:20px;">

The next three code blocks show the three most important use-cases covering the vast majority of typical interaction needed:
- Running the environment through a python script with GUI support
- Running the environment through a python script in Headless mode
- Opening a terminal inside the Docker container using volumes to make operations, like Python packages install, persistent

A typical interaction when using DIAMBRA Arena for Reinforcement Learning research and experimentation could be, for example:

1. Install all dependencies needed to train your Deep RL agent inside the container and make them persistent (i.e. installing Stable-Baselines from the terminal)
2. Prepare your Python training script on your local machine and launch it in headless mode
3. Perform evaluation of your trained agent taking a look at how it behaves running your local Python evaluation script inside the Docker with GUI Support

{{% notice note %}}                                                             
<span style="font-size:20px;">The entire folder where the launcher script is located will be loaded inside the container. The Python script launched needs to be in the same folder of the launcher script.</span>
{{% /notice %}}           

</div>

<h5 style="font-size:20px;" id="random-agent-gui">Random Agent in Dead Or Alive++ (with GUI Support)</h5>

{{< tabs >}}
{{% tab name="Linux (Docker) / MacOS" %}}
```bash
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py -g 1
```
{{% /tab %}}
{{% tab name="Win" %}}
```batch
diambraArena.bat "ROMSPATH=your/roms/local/path" "PYTHONFILE=diambraArenaGist.py" "GUI=1"
```
{{% notice info %}}
Increasing game window size could cause relevant slowdowns to the environments. GUI-supported executions are mainly thought for evaluation purposes. Make sure to use headless mode / deactivate rendering for maximum environment speed (e.g. during training).
{{% /notice %}}
{{% /tab %}}
{{% tab name="Linux (Source)" %}}
```bash
python diambraArenaGist.py --romsPath "your/roms/local/path"
```
{{% /tab %}}
{{< /tabs >}}

<h5 style="font-size:20px;" id="random-agent-headless">Random Agent in Dead Or Alive++ (Headless Mode)</h5>

{{< tabs >}}
{{% tab name="Linux (Docker) / MacOS" %}}
```bash
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py
```
{{% /tab %}}
{{% tab name="Win" %}}
```batch
diambraArena.bat "ROMSPATH=your/roms/local/path" "PYTHONFILE=diambraArenaGist.py"
```
{{% /tab %}}
{{% tab name="Linux (Source)" %}}
Add 
```python
diambraKwargs["headless"] = True
```
to environment keyword arguments in `diambraArenaGist.py` script
```bash
python diambraArenaGist.py --romsPath "your/roms/local/path"
```
{{% /tab %}}
{{< /tabs >}}

<h5 style="font-size:20px;" id="bash-terminal">Bash Terminal With Python Packages Persistent Install</h5>

<div style="font-size:20px;">

In order to make Python packages installation persistent inside the Docker container, Docker volumes are used. The container's Python package folder is linked to a folder in user's local filesystem (named `yourVolumeName`) where all modifications are saved.

</div>

{{< tabs >}}
{{% tab name="Linux (Docker) / MacOS" %}}
```bash
./diambraArena.sh -c bash -v yourVolumeName
```
{{% /tab %}}
{{% tab name="Win" %}}
```batch
diambraArena.bat "CMDTOEXEC=bash" "VOLUME=yourVolumeName"
```
{{% /tab %}}
{{< /tabs >}}

### Advanced usage

#### GPU Docker Image (Linux Only)

{{% notice info %}}
<span style="font-size:20px;">GPU access from docker images is natively supported by Linux hosts only, provided <a href="https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html" target="_blank">Nvidia Container Toolkit</a> has been installed. Windows host systems can support it only via experimental features (for now), as described <a href="https://docs.nvidia.com/cuda/wsl-user-guide/index.html" target="_blank">here</a> and <a href="https://docs.docker.com/desktop/windows/wsl/#gpu-support" target="_blank">here</a>. <b>It is highly recommended to rely on Linux hosts to leverage this feature.</b></span>
{{% /notice %}}

<div style="font-size:20px;">

The next code block aims at verifying that the GPU Docker image has access to the machine GPU. It does so by sending a command to the terminal that: first, installs Tensorflow with PIP, and then executes a short Python script that runs the TF check for GPU availability. It will print "True" in the terminal if GPU can be accessed correctly.

</div>

<h5 style="font-size:20px;" id="gpu-availability">Check GPU Availability with TensorFlow</h5>

```bash
./diambraArena.sh -d GPU -c "pip install tensorflow-gpu==1.14; python -c \"import tensorflow as tf; print('GPU available =', tf.test.is_gpu_available())\""
```

#### Docker Commands Details

<h5 style="font-size:20px;" id="headless-execution">Headless Execution</h5>

```bash
volume="-v ${OPTARG}:/usr/local/lib/python3.6/dist-packages/"
romsPath="--mount src=$romsPath,target="/opt/diambraArena/roms",type=bind "
gpuSetup="--gpus all" 
imageName="diambra:diambra-arena-gpu-cuda10.0"                              
imageName="diambra:diambra-arena-base"

docker run -it --rm $gpuSetup --privileged $volume $romsPath \              
 --mount src=$(pwd),target="/opt/diambraArena/code",type=bind \             
 --name diambraArena $imageName \                                           
  sh -c "cd /opt/diambraArena/code/ && $cmd" 
```

<h5 style="font-size:20px;" id="gui-supported-execution">GUI-Supported Execution</h5>
