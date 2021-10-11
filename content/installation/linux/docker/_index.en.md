---
date: 2016-04-09T16:50:16+02:00
title: Docker (Recommended)
weight: 10
---

## Prerequisites

- Install <a href="https://docs.docker.com/engine/install/#server" target="_blank">Docker Engine</a>
- Install <a href="https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html" target="_blank">Nvidia Container Toolkit</a> (Needed for GPU usage)

## Installation

##### Download examples

- Download <a href="https://github.com/diambra/DIAMBRAenvironment" target="_blank">TODO MODIFY!DIAMBRA Arena Examples</a>, unzip them and navigate to the destination folder in the shell:
    ```bash
    wget XXX; unzip examples.zip && cd examples
    ```

##### Download game ROM(s) and check validity

- Check available games details running:
    ```bash
    ./diambraArena.sh -l
    ```
  Output example:

- Search ROMs and download them. You can use `Search keywords` provided by the game list command reported above, there is a list of suggested terms for each game.
- Check ROM(s) validity 
     ```bash
     ./diambraArena.sh -r "your/roms/local/path" \
                       -t romFileName.zip
     ```

{{% notice tip %}}
Specific game ROM files are required to make DIAMBRA Arena work. Make sure to check ROMs validity of the downloaded file using the above command.
{{% /notice %}}

{{% notice warning %}}
As specified on Terms of Use (Section 8), DIAMBRA Arena is a mere software interface to existing videogames, and it cannot work as a standalone application. As such, it requires the User to own software elements protected by copyright, and to interface them with the Environment itself. It is the case, for example, of Game ROMS required to execute the correspondent Game-related Environment. In such cases, <ins><strong>it is sole and only responsibility of the User to comply with all the laws and regulations, and to make sure he has the right to use such copyright-protected material. DIAMBRA Arena owners will spend their maximum effort in avoiding illegal distribution of such material, and are by no mean responsible for copyright infringement.</strong></ins> 
{{% /notice %}}

## Tests

#### Run random agent in Dead Or Alive++ with GUI support

```bash
./runDocker.sh -r "your/roms/local/path" -s diambraArenaGist.py -g 1
```

#### Validate game ROM file

```python
import diambraArena

# Verify SHA256 Checksum for ROM file
diambraArena.checkGameSha256(gameId, romFile.zip)
```





```bash
./runDocker.sh -h
```
```terminal
Usage:
 
  ./runDocker.sh [OPTIONS]
 
OPTIONS:
  -h Prints out this help message.
 
  -r "<path>" Specify your local path to where game roms are located.
              (Mandatory to run environments.)
 
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
  - Headless (CPU): ./runDocker.sh -r "your/roms/local/path"
                                   -s yourPythonScriptInCurrentDir.py
                                   -v yourVolumeName (optional)
 
  - Headless (GPU): ./runDocker.sh -r "your/roms/local/path"
                                   -s yourPythonScriptInCurrentDir.py
                                   -d GPU
                                   -v yourVolumeName (optional)
 
  - With GUI (CPU): ./runDocker.sh -r "your/roms/local/path"
                                   -s yourPythonScriptInCurrentDir.py
                                   -g 1
                                   -v yourVolumeName (optional)
 
  - Terminal (CPU): ./runDocker.sh -c bash
                                   -v yourVolumeName (optional)
 
  - CUDA Installation Test (GPU): ./runDocker.sh -c "cat /proc/driver/nvidia/version; nvcc -V"
                                                 -d GPU
```
