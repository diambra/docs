---
date: 2016-04-09T16:50:16+02:00
title: Docker (Recommended)
weight: 10
---

## Prerequisites

- Install <a href="https://docs.docker.com/engine/install/#server" target="_blank">Docker Engine</a>
- Install <a href="https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html" target="_blank">Nvidia Container Toolkit</a> (Needed for GPU usage)

## Installation

#### Download DIAMBRA Arena docker images

- Base Image (CPU): `docker pull diambra:diambra-arena-base`

- Image with CUDA Support (GPU - CUDA 10.0): `docker pull diambra:diambra-arena-gpu-cuda10.0`

#### Download examples

- Download <a href="https://github.com/diambra/DIAMBRAenvironment" target="_blank">TODO MODIFY!DIAMBRA Arena Examples</a>, unzip them and open a terminal inside the newly created folder:
    ```bash
    wget XXX; unzip examples.zip && cd examples
    ```

#### Download game ROM(s) and check validity

- Check available games details running:
    ```bash
    ./diambraArena.sh -l
    ```
  Output example:
  ![Test](/images/gamesList.png)

- Search ROMs and download them. You can use <strong>Search keywords</strong> provided by the game list command reported above, there is a list of suggested terms for each game. <strong>Store all ROMs in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`</strong>.

  {{% notice note %}}
  Specific game ROM files are required to make DIAMBRA Arena work. Make sure to check ROMs validity of the downloaded file (next bullet point).
  {{% /notice %}}

- Check ROM(s) validity running:
     ```bash
     ./diambraArena.sh -r "your/roms/local/path" \
                       -t romFileName.zip
     ```
{{% notice tip %}}
To avoid specifying ROMs path every time, you can define a specific environment variable<br> 
A) in your current shell session with: `export DIAMBRAROMSPATH=your/roms/local/path`<br>
B) permanently, adding `DIAMBRAROMSPATH=your/roms/local/path` to the appropriate shell `.profile` (in `bash`, for example, append the line to `~/.bashrc`)
{{% /notice %}}

{{% notice warning %}}
As specified on Terms of Use (Section 8), DIAMBRA Arena is a mere software interface to existing videogames, and it cannot work as a standalone application. As such, it requires the User to own software elements protected by copyright, and to interface them with the Environment itself. It is the case, for example, of Game ROMS required to execute the correspondent Game-related Environment. In such cases, <ins><strong>it is sole and only responsibility of the User to comply with all the laws and regulations, and to make sure he has the right to use such copyright-protected material. DIAMBRA Arena owners will spend their maximum effort in avoiding illegal distribution of such material, and are by no mean responsible for copyright infringement.</strong></ins> 
{{% /notice %}}

## Quickstart 

#### Run random agent in Dead Or Alive++ (with GUI support)

```bash
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py -g 1
```

#### Run random agent in Dead Or Alive++ (Headless mode)

```bash
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py 
```

#### Bash terminal with python package persistent install

```bash
./diambraArena.sh -c bash -v yourVolumeName
```

#### CUDA Installation test
```bash
./diambraArena.sh -c "cat /proc/driver/nvidia/version; nvcc -V" -d GPU
```
#### Check out help message
```bash
./diambraArena.sh -h
```

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

## Advanced Usage

### Commands details

#### Headless Execution


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

#### GUI-Supported Execution
