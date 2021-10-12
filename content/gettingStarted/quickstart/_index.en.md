---
date: 2016-04-09T16:50:16+02:00
title: Quickstart
weight: 10
---

## Assumptions

- Installation completed and succesfully tested as described in <a href="/installation/">Installation</a> section
- <a href="/installation/">TODO: UPDATE LINK Examples</a> provided with DIAMBRA Arena repo dowloaded
- ROMs downloaded and all placed in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`

{{% notice tip %}}
To avoid specifying ROMs for every command you run, you can define a specific environment variable (See <a href="/installation/">Installation</a> section for details). 
{{% /notice %}}

## Basic usage examples

### Script options details (Docker only)

{{< tabs >}}
{{% tab name="Linux (Docker) / MacOS" %}}
```bash
./diambraArena.sh -h
```
Output:
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
Output:

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

### Environment Executions

#### Run random agent in Dead Or Alive++ (with GUI support)

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

#### Run random agent in Dead Or Alive++ (Headless mode)

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

#### Bash terminal with python packages persistent install (using Docker Volumes)

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

## Advanced usage examples

### GPU Docker image executions (Linux only)

{{% notice info %}}
GPU access from docker images is natively supported by Linux hosts only, provided <a href="https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html" target="_blank">Nvidia Container Toolkit</a> has been installed. Windows host systems can support it only via experimental features (for now), as described <a href="https://docs.nvidia.com/cuda/wsl-user-guide/index.html" target="_blank">here</a> and <a href="https://docs.docker.com/desktop/windows/wsl/#gpu-support" target="_blank">here</a>. <b>It is highly recommended to rely on Linux hosts to leverage this feature.</b> 
{{% /notice %}}

#### Check GPU availability with TensorFlow

```bash
./diambraArena.sh -d GPU -c "pip install tensorflow-gpu==1.14; python -c \"import tensorflow as tf; print('GPU available =', tf.test.is_gpu_available())\""
```

### Docker commands details

#### Headless execution

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

#### GUI-supported execution
