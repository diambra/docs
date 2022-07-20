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

DIAMBRA Arena comes with a very handy tool: the DIAMBRA Command Line Interface (DIAMBRA CLI). It provides different useful commands, with related options, that contribute to make running DIAMBRA Arena environments super easy.

The main use of the CLI is running a command after brining up DIAMBRA Arena containerized environment(s). It sets the `DIAMBRA_ENVS` environment variable to list the endpoints of all running environments.

Usage:

```shell
diambra run [flags] <command-to-execute>
```

The only flag needed for simple executions is listed below. Advanced usage and options can be found in the <a href="./#diambra-cli-advanced-options">CLI Advanced Options</a> section below.

| <strong><span style="color:#5B5B60;">Flag</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>
|-------------|-------------| ------|
| `-r, --path.roms`      | `string` | Path to ROMs (default to DIAMBRAROMSPATH env var if set) |

[^1]: Currently available only for Linux systems. Successfully trained Deep RL Agent in Single Player mode.

##### Script Execution

To run a python script using the CLI, one can just execute the following command:

```shell
diambra run -r /path/to/roms/folder/ python diambra_arena_gist.py
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

The table below lists all available options for this command.

| <strong><span style="color:#5B5B60;">Flag</span></strong> | <strong><span style="color:#5B5B60;">Type</span></strong> | <strong><span style="color:#5B5B60;">Description</span></strong>
|-------------|-------------| ------|
| `-h, --help`              | -        | Help for run command |
| `-r, --path.roms`         | `string` | Path to ROMs (default to DIAMBRAROMSPATH env var if set) |
| `-g, --engine.render*` | -        | Render graphics server side |
| `-l, --engine.lockfps`    | -        | Lock FPS |
| `-n, --engine.sound`      | -        | Enable sound |
| `-s, --env.scale`         | `int`        | Number of environments to run (default 1) |
| `--path.credentials`  | `string` | Path to credentials file (default "$HOME/.diambra/credentials") |
| `-e, --env.image`   | `string`   | Env image to use (default "diambra/engine:main") |
| `-p, --images.pull`   | `bool`   | (Always) pull image before running (default true) |
| `--env.mount`   | `string`        | Host mounts for env container (/host/path:/container/path) |

`*`: Currently available only for Linux systems. For additional info and for Windows/MacOS alternatives, see <a href="./#environment-native-rendering">Environment Native Rendering</a> section below.

###### Arena Command

Usage:

```shell
diambra arena [flags] [command]
```

It brings up DIAMBRA Arena containerized environment(s) and returns to the terminal output the endpoints of all running environments.

The table below lists all available commands for this mode.

Flags reported for the Run command above apply also to this mode.

| <strong><span style="color:#5B5B60;">Command</span></strong> |  <strong><span style="color:#5B5B60;">Description</span></strong>
|-------------|------|
| `down` | Stop DIAMBRA Arena container(s)|
| `up`   | Start DIAMBRA Arena container(s) |

##### Using Python Notebooks

DIAMBRA Arena can also be used inside python notebooks. There are two options to do it, explained here after.

The most straightfoward one is to launch Jupyter Notebook through the CLI as shown by the next command:

```shell
diambra run jupyter notebook
```

This step is needed to boot up the environment container and load its connection port in the `DIAMBRA_ENVS` environment variable. This variable is thus accessible from within Jupyter Notebook, that looks like the following one:

{{< pybook diambraarenagist 700 >}}

If one wants to execute a notebook from somewhere else (for example from inside Visual Studio Code), it is not possible to leverage the `DIAMBRA_ENVS` environment variable for passing the connection port information. In such cases, one should activate the environment container and retrieve its port, and this can be done by means of the CLI command `diambra arena up` that returns the port address, as shown below:

```shell
diambra arena up
Server listening on 0.0.0.0:50051
127.0.0.1:49153
```

This information is then passed to the make function of DIAMBRA Arena through the setting `"env_address"` as shown in the next jupyter notebook:

{{< pybook diambraarenagist2 700 >}}

Once done, one can stop running container(s) as follows:

```shell
diambra arena down
(bb1a) stopping container
```

##### Environment Native Rendering

It is possible to activate emulator native rendering while running environments (i.e. bringing up the emulator graphics window). The CLI provides a specific flag for this purpose, but currently this is supported only on Linux, while Windows and MacOS users have to configure a Xserver and link it to the environment container. The next tabs provide hints for each context.

{{< tabs groupId="linuxSource">}}
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
{{% tab name="MacOs" %}}


To run environments with native emulator GUI support on MacOS, currently requires the user to setup a virtual XServer and connect it to the container. We cannot provide support for this use case at the moment, but we plan to implement this feature in the near future.

A virtual XServer that in our experience proved to be effective is <a href="https://www.xquartz.org/releases/XQuartz-2.7.8.html" target="_blank">XQuartz 2.7.8</a> coupled to `socat` that can be installed via `brew install socat`.

{{% /tab %}}
{{< /tabs >}}

##### Running Multiple Environments in Parallel

