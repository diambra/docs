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
  - <a href="./#running-multiple-environments-in-parallel">Running Multiple Environments in Parallel</a>
  - <a href="./#run-diambra-engine-without-cli">Run DIAMBRA Engine without CLI</a>
  - <a href="./#environment-native-rendering">Environment Native Rendering</a>

</div>

### Prerequisites

- Having completed and tested the installation as described in <a href="../#installation">Installation</a> and <a href="../#quickstart">Quickstart</a> sections in homepage
- Having downloaded the ROMs and placed them all in the same folder, whose absolute path will be referred in the following as `/absolute/path/to/roms/folder/`

{{% notice tip %}}
To avoid specifying ROMs path at every run, you can define the environment variable `DIAMBRAROMSPATH=/absolute/path/to/roms/folder/`, either temporarily in your current shell/prompt session, or permanently in your profile (e.g. on linux in `~/.bashrc`).
{{% /notice %}}

### Running the Environment

##### Basic Script

The most straightforward and simple script to use DIAMBRA Arena is reported below. It features a random agent playing Dead Or Alive ++, and it represents the general interaction schema to be used for every game and context of DIAMBRA Arena.

{{< github_code "https://raw.githubusercontent.com/diambra/arena/main/examples/diambra_arena_gist.py" >}}

{{% notice note %}}
More complex and complete examples can be found in the <a href="./examples/">Examples</a> section.
{{% /notice %}}

##### DIAMBRA Command Line Interface (CLI)

DIAMBRA Arena comes with a very handy tool: the DIAMBRA Command Line Interface (DIAMBRA CLI). It provides different useful commands, with related options, that contribute to make running DIAMBRA Arena environments super easy.

The main use of the CLI is running a command after brining up DIAMBRA Arena containerized environment(s). It sets the `DIAMBRA_ENVS` environment variable to list the endpoints of all running environments.

Usage:

```shell
diambra run [flags] <command-to-execute>
```

The only flag needed for simple executions is listed below. Advanced usage and options can be found in the <a href="./#diambra-cli-advanced-options">CLI Advanced Options</a> section below.

| <strong><span style="color:#5B5B60;">Flag</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong> |
| --------------------------------------------------------- | --------------------------------------------------------- | ---------------------------------------------------------------- |
| `-r, --path.roms`                                         | `str`                                                     | Path to ROMs (default to DIAMBRAROMSPATH env var if set)         |

[^1]: Currently available only for Linux systems. Successfully trained Deep RL Agent in Single Player mode.

##### Script Execution

To run a python script using the CLI, one can just execute the following command:

```shell
diambra run -r /absolute/path/to/roms/folder/ python diambra_arena_gist.py
```

This will start a new container with the environment, load in the `DIAMBRA_ENVS` environment variable the port on which the environment accepts connections, and run the script where the DIAMBRA Arena python module is imported and used to instantiate a new environment, that will automatically retrieve the port and connect to it.

### Advanced Usage

{{% notice note %}}
In what follows, we will omit the `-r` flag for the CLI, assuming the user has set the `DIAMBRAROMSPATH` environment variable in his system.
{{% /notice %}}

##### DIAMBRA CLI Advanced Options

###### Run Command

Usage:

```shell
diambra run [flags] <command-to-execute>
```

It runs a command after brining up DIAMBRA Arena containerized environment(s). It sets the `DIAMBRA_ENVS` environment variable to list the endpoints of all running environments.

The next snippet shows the help message for this command, where all available options are reported:

```shell
Run runs the given command after diambraEngine is brought up.

It will set the DIAMBRA_ENVS environment variable to list the endpoints of all running environments.
The DIAMBRA arena python package will automatically be configured by this.

The flag --agent-image can be used to run the commands in the given image.

Usage:
  diambra run [flags] command [args...]

Flags:
  -a, --agent.image string        Run given agent command in container
  -l, --engine.lockfps            Lock FPS
  -g, --engine.render             Render graphics server side
      --engine.sound              Enable sound
  -x, --env.autoremove            Remove containers on exit (default true)
      --env.containerip           Use <containerIP>:<containerPort> instead of <env.host/localhost>:<hostPort>
      --env.host string           Host to bind ports on (default "127.0.0.1")
      --env.image string          Env image to use, omit to detect from diambra-arena version
      --env.mount strings         Host mounts for env container (/host/path:/container/path)
      --env.preallocateport       Preallocate port for env container. Workaround for port conflicts on Windows
  -s, --env.scale int             Number of environments to run (default 1)
      --env.seccomp string        Path to seccomp profile to use for env (may slow down environment). Set to "" for runtime's default profile. (default "unconfined")
  -h, --help                      help for run
  -n, --images.no-pull            Do not try to pull image before running
      --init.image string         Init image to use (default "ghcr.io/diambra/init:main")
  -i, --interactive               Open stdin for interactions with arena and agent (default true)
      --path.credentials string   Path to credentials file (default "/home/alexpalms/.diambra/credentials")
  -r, --path.roms string          Path to ROMs (default to DIAMBRAROMSPATH env var if set) (default "/home/alexpalms/work/diambra/roms")

Global Flags:
  -d, --log.debug           Enable debug logging
      --log.format string   Set logging output format (logfmt, json, fancy) (default "fancy")
```

{{% notice note %}}
Currently, the `-g, --engine.render` option to render graphics server side, is only available for Linux systems. For additional info and for Windows/MacOS alternatives, see <a href="./#environment-native-rendering">Environment Native Rendering</a> section below.
{{% /notice %}}

###### Arena Command

Usage:

```shell
diambra arena [flags] [command]
```

It brings up DIAMBRA Arena containerized environment(s) and returns to the terminal output the endpoints of all running environments.

The snippet below lists all available commands for this mode.

Flags reported for the Run command above apply also to this mode.

```shell
These are the arena related commands

Usage:
  diambra arena [command]

Available Commands:
  check-roms  check roms
  down        Stop DIAMBRA Arena
  list-roms   list roms
  status      Show status of DIAMBRA arena
  up          Start DIAMBRA arena
  version     version

Flags:
  -h, --help   help for arena

Global Flags:
  -d, --log.debug           Enable debug logging
      --log.format string   Set logging output format (logfmt, json, fancy) (default "fancy")

Use "diambra arena [command] --help" for more information about a command.
```

##### Running Multiple Environments in Parallel

It can be useful to run multiple environment instances in parallel, for example for Deep RL training. The CLI provides a flag to control this, it can be used both by the `run` and the `arena` commands. The former will load a string in the `DIAMBRA_ENVS` environment variable where connection addresses are listed and separated by a space, while the latter will print out in the terminal the same string. These values can then be used properly to setup multiple parallel connections.

Running a script after having started 16 containers:

```
diambra run -s=16 python training_script.py
```

Starting 4 containers and printing their addresses in the terminal:

```
diambra arena -s=4 up
Server listening on 0.0.0.0:50051
127.0.0.1:49154 127.0.0.1:49155 127.0.0.1:49156 127.0.0.1:49157
```

##### Run DIAMBRA Engine without CLI

Agents connect via network using gRPC to DIAMBRA Engine running in a Docker container. The `diambra` CLI's `run` command starts the DIAMBRA Engine in a Docker container and sets up the environment to make it easy to connect to the Engine. For troubleshooting it might be useful to run the Engine manually, using host networking.

###### Start Engine

{{< tabs groupId="manualEngineDocker">}}
{{% tab name="Linux/MacOS" %}}

```bash
mkdir ~/.diambra
touch ~/.diambra/credentials

docker run -d --rm --name engine \
  -v $HOME/.diambra/credentials:/tmp/.diambra/credentials \
  -v /absolute/path/to/roms/folder/:/opt/diambraArena/roms \
  -p 127.0.0.1:50051:50051 \ 
  docker.io/diambra/engine:latest
```

{{% /tab %}}
{{% tab name="Win (cmd)" %}}

```cmd
mkdir %userprofile%/.diambra
echo > %userprofile%/.diambra/credentials

docker run --rm -ti --name engine ^
  -v %userprofile%/.diambra/credentials:/tmp/.diambra/credentials ^
  -v %userprofile%/.diambra/roms:/opt/diambraArena/roms ^
  -p 127.0.0.1:50051:50051 ^
  docker.io/diambra/engine:latest
```

{{% /tab %}}
{{% tab name="Win (PowerShell)" %}}

```powershell
mkdir $Env:userprofile/.diambra
echo "" > $Env:userprofile/.diambra/credentials

docker run --rm -ti --name engine `
  -v $Env:userprofile/.diambra/credentials:/tmp/.diambra/credentials `
  -v $Env:userprofile/.diambra/roms:/opt/diambraArena/roms `
  -p 127.0.0.1:50051:50051 `
  docker.io/diambra/engine:latest
```
{{% /tab %}}
{{< /tabs >}}

{{% notice note %}}
Creating the `~/.diambra` and `~/.diambra/credentials` is only needed when you never ran the diambra CLI before. Otherwise this step can be skipped.
{{% /notice %}}

###### Connect to Engine

Now you can run the script that uses DIAMBRA Arena by opening a new terminal and setting `DIAMBRA_ENVS` environment variable followed by the python command:

```bash
DIAMBRA_ENVS=localhost:50051 python ./script.py
```

##### Environment Native Rendering

It is possible to activate emulator native rendering while running environments (i.e. bringing up the emulator graphics window). The CLI provides a specific flag for this purpose, but currently this is supported only on Linux, while Windows and MacOS users have to configure a Xserver and link it to the environment container. The next tabs provide hints for each context.

{{< tabs groupId="nativeRendering">}}
{{% tab name="Linux" %}}

On Linux, the CLI allows to render emulator natively on the host, the user only needs to add the `-g` flag to the run command, as follows:

```shell
diambra run -g python diambra_arena_gist.py
```

{{% notice note %}}
Activating emulator native rendering will open a GUI where the game executes. Currently, this feature is affected by a problem: the mouse cursor disappears and remains constrained inside such window. To re-aquire control of the OS Xserver, one can circle through the active windows using the key combination ALT+TAB and highlight a different one.
{{% /notice %}}

{{% /tab %}}
{{% tab name="Win" %}}

To run environments with native emulator GUI support on Windows, currently requires the user to setup a virtual XServer and connect it to the container. We cannot provide support for this use case at the moment, but we plan to implement this feature in the near future.

A virtual XServer that in our experience proved to be effective is <a href="https://sourceforge.net/projects/vcxsrv/" target="_blank">VcXsrv Windows X Server</a>.

{{% /tab %}}
{{% tab name="MacOS" %}}

To run environments with native emulator GUI support on MacOS, currently requires the user to setup a virtual XServer and connect it to the container. We cannot provide support for this use case at the moment, but we plan to implement this feature in the near future.

A virtual XServer that in our experience proved to be effective is <a href="https://www.xquartz.org/releases/XQuartz-2.7.8.html" target="_blank">XQuartz 2.7.8</a> coupled to `socat` that can be installed via `brew install socat`.

{{% /tab %}}
{{< /tabs >}}