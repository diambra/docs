---
title: Getting Started
weight: 20
---

### Index

<div style="font-size:1.125rem;">

- <a href="./#prerequisites">Prerequisites</a>
- <a href="./#running-the-environment">Running the Environment</a>
    - <a href="./#basic-script">Basic Script</a>
    - <a href="./#diambra-command-line-interface-cli">DIAMBRA Command Line Interface (CLI)</a>
    - <a href="./#script-execution">Script Execution</a>
- <a href="./#advanced-usage">Advanced Usage</a>
    - <a href="./#diambra-cli-advanced-options">DIAMBRA CLI Advanced Options</a>
    - <a href="./#using-python-notebooks">Using Python Notebooks</a>
    - <a href="./#environment-native-rendering">Environment Native Rendering</a>
    - <a href="./#running-multiple-environments-in-parallel">Running Multiple Environments in Parallel</a>

</div>

### Prerequisites

- Installation completed and tested as described in <a href="/#installation">Installation</a> and <a href="/#quickstart">Quickstart</a> homepage sections
- <a href="https://github.com/diambra/arena/tree/main/examples" target="_blank">Examples</a> provided with DIAMBRA Arena repo downloaded
- ROMs downloaded and placed all in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`

{{% notice tip %}}
To avoid specifying ROMs path at every run, you can define the environment variable `DIAMBRAROMSPATH=your/roms/local/path`, either temporarily in your current shell/prompt session, or permanently in your profile (e.g. on linux in `~/.bashrc`).
{{% /notice %}}

### Running the Environment

##### Basic Script

The most straightforward and simple script to use DIAMBRA Arena is reported below (also found in the examples as `diambra_arena_gist.py`). It features a random agent playing Dead Or Alive ++, but it represents the general interaction shema to be used for every game and context of DIAMBRA Arena.

```python {linenos=inline}
 # DIAMBRA Arena module import
 import diambra.arena

 # Environment creation
 env = diambra.arena.make("doapp")

 # Environment reset
 observation = env.reset()

 # Agent-Environment interaction loop
 while True:
     # (Optional) Environment rendering
     env.render()

     # Action random sampling
     actions = env.action_space.sample()

     # Environment stepping
     observation, reward, done, info = env.step(actions)

     # Episode end (Done condition) check
     if done:
         observation = env.reset()
         break

 # Environment close
 env.close()
```

{{% notice note %}}
More complex and complete examples can be found in the <a href="./examples/">Examples</a> section.
{{% /notice %}}

##### DIAMBRA Command Line Interface (CLI)

To allow a very smooth execution, we included a *launcher script* in DIAMBRA Arena repo, inside the <a href="https://github.com/diambra/diambraArena/tree/main/examples" target="_blank">Examples</a> folder. It is a bash/batch script named `diambraArena.sh`/`diambraArena.bat` for Linux-MacOS/Windows respectively.

It provides a simple interface to easily perform many useful tasks, like:
 - Validating a ROM file
 - Executing a local python script, with and without GUI support (X-server)
 - Executing a terminal command inside the Docker container
 - Opening a live terminal inside the Docker container with optional support for persistent update of the container itself
 - Running the CPU only or the GPU container version (Linux Only)

The next block shows how to print out all available options for every OS.

##### Script Execution

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

### Advanced Usage

##### DIAMBRA CLI Advanced Options

##### Using Python Notebooks

{{< pybook diambraarenagist 700 >}}

##### Environment Native Rendering

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

##### Running Multiple Environments in Parallel

