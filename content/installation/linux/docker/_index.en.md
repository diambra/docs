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
     ./diambraArena.sh -r "your/roms/local/path" -t romFileName.zip
     ```
{{% notice tip %}}
To avoid specifying ROMs path for every command you run, you can define a specific environment variable<br> 
A) in your current shell session with: `export DIAMBRAROMSPATH=your/roms/local/path`<br>
B) permanently, adding `DIAMBRAROMSPATH=your/roms/local/path` to the appropriate shell `.profile` (in `bash`, for example, append the line to `~/.bashrc`)
{{% /notice %}}

{{% notice warning %}}
As specified on Terms of Use (Section 8), DIAMBRA Arena is a mere software interface to existing videogames, and it cannot work as a standalone application. As such, it requires the User to own software elements protected by copyright, and to interface them with the Environment itself. It is the case, for example, of Game ROMS required to execute the correspondent Game-related Environment. In such cases, <ins><strong>it is sole and only responsibility of the User to comply with all the laws and regulations, and to make sure he has the right to use such copyright-protected material. DIAMBRA Arena owners will spend their maximum effort in avoiding illegal distribution of such material, and are by no mean responsible for copyright infringement.</strong></ins> 
{{% /notice %}}

## Installation Tests 

#### Run random agent in Dead Or Alive++ (Headless mode)

```bash
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py 
```

#### CUDA Installation test
```bash
./diambraArena.sh -c "cat /proc/driver/nvidia/version; nvcc -V" -d GPU
```
