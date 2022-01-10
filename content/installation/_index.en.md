---
title: Installation
weight: 10
---

<div style="font-size:1.125rem;">

### Index

- <a href="./#overview">Overview</a>
- <a href="./#prerequisites">Prerequisites</a>
- <a href="./#installation">Installation</a>
- <a href="./#installation-tests">Installation Tests</a>

</div>

### Overview

DOCKER VS FROM SOURCE

GPU Support

### Prerequisites

{{% notice note %}}
Running environment
{{% /notice %}}

{{< tabs groupId="installationTabs">}}
{{% tab name="Linux (Docker)" %}}

- Install <a href="https://docs.docker.com/engine/install/#server" target="_blank">Docker Engine</a>
- Install <a href="https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html" target="_blank">Nvidia Container Toolkit</a> (Needed for GPU usage)

{{% /tab %}}
{{% tab name="Windows" %}}

- Install <a href="https://docs.docker.com/desktop/windows/install/" target="_blank">Docker Desktop for Windows</a>
- Install <a href="https://sourceforge.net/projects/vcxsrv/" target="_blank">VcXsrv Windows X Server</a>

{{% /tab %}} 
{{% tab name="MacOS" %}}

- Install <a href="https://docs.docker.com/desktop/mac/install/" target="_blank">Docker Desktop for MacOS</a>
- Install socat:
  ```bash
  brew install socat
  ```
- Install <a href="https://www.xquartz.org/releases/XQuartz-2.7.8.html" target="_blank">Xquartz 2.7.8</a>

{{% /tab %}} 
{{% tab name="Linux (Source)" %}}

##### Operating System

- Linux Mint 19 or newer
- Linux Ubuntu 18.04 or newer

##### Virtual Environment (Optional, but strongly suggested)

Install and use Virtual Environment to manage dependencies, both <a href="https://virtualenv.pypa.io/en/latest/" target="_blank">VirtualEnv</a> and <a href="https://docs.conda.io/projects/conda/en/latest/index.html" target="_blank">[Ana]Conda</a> are good options.

{{% /tab %}}
{{< /tabs >}}  

### Installation

{{< tabs groupId="installationTabs">}}
{{% tab name="Linux (Docker)" %}}

##### Download DIAMBRA Arena Docker Images

- Base Image (CPU):
  ```bash
  docker pull diambra:diambra-arena-base
  ```

- Image with CUDA Support (GPU - CUDA 10.0):
  ```bash
  docker pull diambra:diambra-arena-gpu-cuda10.0
  ```

##### Download Examples

- Download <a href="https://github.com/diambra/DIAMBRAenvironment" target="_blank">TODO MODIFY!DIAMBRA Arena Examples</a>, unzip them and open a terminal inside the newly created folder:
    ```bash
    wget XXX; unzip examples.zip && cd examples
    ```

##### Download Game ROM(s) and Check Validity

- Check available games details running:
    ```bash
    ./diambraArena.sh -l
    ```
  Output example:
<figure style="margin-right:auto; margin-left:auto;">
  <img src="../images/gamesList.png" style="padding-left:40px;margin-bottom:3rem; margin-top:0px">
</figure>  

- Search ROMs and download them. You can use <span style="color:#333333; font-weight:bolder;">Search Keywords</span> provided by the game list command reported above, there is a list of suggested terms for each game. <span style="color:#333333; font-weight:bolder;">Store all ROMs in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`</span>.

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

{{% /tab %}}
{{% tab name="Windows" %}}

##### Download DIAMBRA Arena Docker Images

- Base Image (CPU): 
  ```cmd
  docker pull diambra:diambra-arena-base
  ```

##### Download Examples

- Download <a href="https://github.com/diambra/DIAMBRAenvironment" target="_blank">TODO MODIFY!DIAMBRA Arena Examples</a>, unzip them and open a terminal inside the newly created folder

##### Download Game ROM(s) and Check Validity

- Check available games details running:
    ```bat
    diambraArena.bat -l
    ```
  Output example:
<figure style="margin-right:auto; margin-left:auto;">
  <img src="../images/gamesList.png" style="padding-left:40px;margin-bottom:3rem; margin-top:0px">
</figure>  

- Search ROMs and download them. You can use <span style="color:#333333; font-weight:bolder;">Search Keywords</span> provided by the game list command reported above, there is a list of suggested terms for each game. <span style="color:#333333; font-weight:bolder;">Store all ROMs in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`</span>.

  {{% notice note %}}
  Specific game ROM files are required to make DIAMBRA Arena work. Make sure to check ROMs validity of the downloaded file (next bullet point).
  {{% /notice %}}

- Check ROM(s) validity running:
  ```bat
  diambraArena.bat "ROMSPATH=your\roms\local\path" "ROMCHECK=<romFile>.zip"
  ```
{{% notice tip %}}
To avoid specifying ROMs path for every command you run, you can define a specific environment variable named `DIAMBRAROMSPATH`.<br> 
{{% /notice %}}

{{% /tab %}} 
{{% tab name="MacOS" %}}

##### Download DIAMBRA Arena Docker Images

- Base Image (CPU):
  ```bash
  docker pull diambra:diambra-arena-base
  ```

##### Download Examples

- Download <a href="https://github.com/diambra/DIAMBRAenvironment" target="_blank">TODO MODIFY!DIAMBRA Arena Examples</a>, unzip them and open a terminal inside the newly created folder:
    ```bash
    wget XXX; unzip examples.zip && cd examples
    ```

##### Download Game ROM(s) and Check Validity

- Check available games details running:
    ```bash
    ./diambraArena.sh -l
    ```
  Output example:
<figure style="margin-right:auto; margin-left:auto;">
  <img src="../images/gamesList.png" style="padding-left:40px;margin-bottom:3rem; margin-top:0px">
</figure>  

- Search ROMs and download them. You can use <span style="color:#333333; font-weight:bolder;">Search Keywords</span> provided by the game list command reported above, there is a list of suggested terms for each game. <span style="color:#333333; font-weight:bolder;">Store all ROMs in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`</span>.

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

{{% /tab %}} 
{{% tab name="Linux (Source)" %}}

##### Install OS Dependencies and Python Packages

- Download or clone <a href="" target="_blank">TODO UPDATE LINKDIAMBRA Arena Repository</a>
- Open a terminal and navigate inside the repository root
- Execute the installation script to install OS dependencies
  ```bash
  ./setupOS.sh
  ```
- Install the DIAMBRA Arena with PIP
  ```bash
  pip3 install .
  ```
##### Download Game ROM(s) and Check Validity

- Check available games details running:

    ```bash
    python -c "import diambraArena; diambraArena.availableGames(True, True)"
    ```
  Output example:
<figure style="margin-right:auto; margin-left:auto;">
  <img src="../images/gamesList.png" style="padding-left:40px;margin-bottom:3rem; margin-top:0px">
</figure>  

- Search ROMs and download them. You can use <span style="color:#333333; font-weight:bolder;">Search Keywords</span> provided by the game list command reported above, there is a list of suggested terms for each game. <span style="color:#333333; font-weight:bolder;">Store all ROMs in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`</span>.

  {{% notice note %}}
  Specific game ROM files are required to make DIAMBRA Arena work. Make sure to check ROMs validity of the downloaded file (next bullet point).
  {{% /notice %}}

- Check ROM(s) validity running:
  ```bash
  python -c "import diambraArena, os; diambraArena.checkGameSha256(os.path.join('your/roms/local/path', 'romFileName.zip'))"
  ```
{{% notice tip %}}
To avoid specifying ROMs path for every command you run, you can define a specific environment variable<br> 
A) in your current shell session with: `export DIAMBRAROMSPATH=your/roms/local/path`<br>
B) permanently, adding `DIAMBRAROMSPATH=your/roms/local/path` to the appropriate shell `.profile` (in `bash`, for example, append the line to `~/.bashrc`)
{{% /notice %}}

{{% /tab %}}
{{< /tabs >}}  

{{% notice warning %}}
As specified on Terms of Use (Section 8), DIAMBRA Arena is a mere software interface to existing videogames, and it cannot work as a standalone application. As such, it requires the User to own software elements protected by copyright, and to interface them with the Environment itself. It is the case, for example, of Game ROMS required to execute the correspondent Game-related Environment. In such cases, <ins><span style="color:#333333; font-weight:bolder;">it is sole and only responsibility of the User to comply with all the laws and regulations, and to make sure he has the right to use such copyright-protected material. DIAMBRA Arena owners will spend their maximum effort in avoiding illegal distribution of such material, and are by no mean responsible for copyright infringement.</span></ins> 
{{% /notice %}}

### Installation Tests 

{{< tabs groupId="installationTabs">}}
{{% tab name="Linux (Docker)" %}}

##### Run Random Agent in Dead Or Alive++ (Headless Mode)

```bash
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py 
```

##### CUDA Installation Test

```bash
./diambraArena.sh -c "cat /proc/driver/nvidia/version; nvcc -V" -d GPU
```
{{% /tab %}}
{{% tab name="Windows" %}}

##### Run Random Agent in Dead Or Alive++ (Headless Mode)

```bat
diambraArena.bat "ROMSPATH=your/roms/local/path" "PYTHONFILE=diambraArenaGist.py" 
```
{{% /tab %}} 
{{% tab name="MacOS" %}}

##### Run Random Agent in Dead Or Alive++ (Headless Mode)

```bash
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py 
```


{{% /tab %}} 
{{% tab name="Linux (Source)" %}}

##### Run Random Agent in Dead Or Alive++

Navigate inside the Examples folder provided by DIAMBRA Arena Repo and execute the basic the Python script as follows:

```bash
python diambraArenaGist.py --romsPath "your/roms/local/path" 
```   

{{% /tab %}}
{{< /tabs >}}  
