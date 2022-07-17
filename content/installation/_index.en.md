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

DIAMBRA Arena runs on all major Operating Systems: Linux, Windows and MacOS. It is natively supported on Linux Ubuntu 18.04 or newer and Linux Mint 19 or newer, where it can be installed directly from sources. For all other options (different Linux distributions, Windows and MacOS), it leverages Docker technology. 

Docker installation is quick and controlled, with equal (if not better) performances in terms of environment execution speed, when compared with installation from source. It even allows, on every OS, to run all environments with rendering active, showing game frames on screen in real time.

The only Docker limitation is for Windows and MacOS users: at the moment it does not support host machine's GPU interfacing, impeding to leverage CUDA computing. To do so within Docker, one needs to use Linux-based hosts. Still, it does not represent a road-block for Windows and MacOS users, as typical Deep RL networks are shallow, allowing to perform CPU-based training and inference with only minor (if any) slowdowns.

Ubuntu / Mint users can follow both installation paths (Docker-based / Source-based), each one with pros and cons. Using Docker allows to levearge a well isolated container able to even speed up executions in some cases, at the price of requiring a little bit of experience and overhead to properly handle aspects like folder sharing/binding, persistent container modifications, and similar ones. Installing DIAMBRA Arena from source allows to have deeper access to code, making easier to interface it with custom libraries, tools or scripts. It requires, though, to manually handle aspects as making sure all dependencies are installed and kept aligned in time. 

### Installation Test

##### 3) Download examples

- Download <a href="https://github.com/diambra/arena/tree/main/examples" target="_blank">DIAMBRA Arena Examples</a>, unzip them and open a terminal inside the newly created folder


{{< tabs groupId="installationTabs">}}
{{% tab name="Linux" %}}

##### Run Random Agent in Dead Or Alive++ (Headless Mode)

Navigate inside the Examples folder provided with <a href="https://github.com/diambra/diambraArena" target="_blank">DIAMBRA Arena repository</a>, where launcher and Python scripts are located. <span style="color:#333333; font-weight:bolder;">Your <a href="https://diambra.ai/register/" target="_blank">DIAMBRA credentials</a> will be asked at the first execution.</span>

Run the environment as shown below:

```shell
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py 
```

A successful installation would result in a terminal printout like the following (note that it takes up to a few tens of seconds to complete a round):

```shell
EnvId = TestEnv
Action Spaces = multiDiscrete
Use attack buttons combinations = [True, True]
diambraEnv library successfully loaded

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
      .:-:-**#*#####+***+=-:. 
 ..-++####+#################=+=:. 
:+*#*###########################*-. 
   .-+#############################=. 
      .-*###########################+. 
        .=######++======++*#########*#=. ........ ...     .......     ...........     ........     ........ ............     ............         ........... 
          -*=------:---------=*########- .------..-----:. .------.   .-----------.    --------.   :-------- --------------:. --------------:.    :----------: 
          .:--------:---------:=#######* .------..-------..------.   :-----:-----:.   ---------. .--------- ------:..:-----: ------:..------:   .------:-----. 
        .:------::---::--------:+#######..------. .------..------.  .------.:-----.   ----------.---------- ------:..------. ------:  :-----.  .:-----:.------. 
        :-----::.:---:-----::---:######* .------. .------..------. .:-----: .------.  ------:-------------- ------::-----:.. ------:.-----:.   .------..------. 
       .--:..  . :::.:.---.-:---.#####*- .------. .------..------. .------.::------:  ------:.-----.:------ ------:  :-----: ------:.:------: .------:.:-------. 
        ..:..:::.:::.::....:.::--:####*. .------::------:..------..------:.:::------. ------: .---. :-----: -------::------: ------:  ------: :------..::------: 
       .-::::::---...:--. .-.:-::=###+.  .::::::::::::..  .::::::..::::::.   .::::::. ::::::.  .:.  ::::::: ::::::::::::::.  :::::::  ::::::: ::::::.    .:::::: 
       .--:.   .:--...::. ..::+####*: 
       :--:..   .---:...:.   .:+*+:. 
       .:---:::----.. ....    .:. 
        .:------:..:...  :. 
         .......   ..:.  .. 


                                                                   DIAMBRA™ | Dueling AI Arena
                                                              https://diambra.ai - info@diambra.ai

                                                         DIAMBRA™ is a Trade Mark, © Copyright 2018-2022
                                                                              v. 1.0
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

Environment initialization ...
SHA256 check ok. Correct rom file found.
78081g503.ic655 NOT FOUND (NO GOOD DUMP KNOWN) (tried in doapp coh1002m doapp)
WARNING: the machine might not run correctly.
2188 Completed console init
Num. of Channels = 3
Screen Dim (W x H) = 512 480
(0)Buttons configuration:
(0)  H = But6
(0)  K = But2
(0)  P = But1
(0)Game Continue Val = 0
(0)Show final = 1
(0)1P Environment
(0)Player side = Random
(0)Characters = [Random, Random]
(0)Number of outfits = [2, 2]
done.
Using MultiDiscrete action space
(0)Setting difficulty = 3
(0)Starting game
(0)Player starting side = P2
(0)P2 = Kasumi
(0)Waiting for fight to start
(0)Round done
(0)Moving to next round
```

{{% /tab %}}
{{% tab name="Win" %}}

##### Run Random Agent in Dead Or Alive++ (Headless Mode)

Navigate inside the Examples folder provided with <a href="https://github.com/diambra/diambraArena" target="_blank">DIAMBRA Arena repository</a>, where launcher and Python scripts are located. <span style="color:#333333; font-weight:bolder;">Your <a href="https://diambra.ai/register/" target="_blank">DIAMBRA credentials</a> will be asked at the first execution.</span>

Run the environment as shown below:

```shell
diambraArena.bat "ROMSPATH=your/roms/local/path" "PYTHONFILE=diambraArenaGist.py" 
```

A successful installation would result in a terminal printout like the following (note that it takes up to a few tens of seconds to complete a round):

```shell
EnvId = TestEnv
Action Spaces = multiDiscrete
Use attack buttons combinations = [True, True]
diambraEnv library successfully loaded

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
      .:-:-**#*#####+***+=-:. 
 ..-++####+#################=+=:. 
:+*#*###########################*-. 
   .-+#############################=. 
      .-*###########################+. 
        .=######++======++*#########*#=. ........ ...     .......     ...........     ........     ........ ............     ............         ........... 
          -*=------:---------=*########- .------..-----:. .------.   .-----------.    --------.   :-------- --------------:. --------------:.    :----------: 
          .:--------:---------:=#######* .------..-------..------.   :-----:-----:.   ---------. .--------- ------:..:-----: ------:..------:   .------:-----. 
        .:------::---::--------:+#######..------. .------..------.  .------.:-----.   ----------.---------- ------:..------. ------:  :-----.  .:-----:.------. 
        :-----::.:---:-----::---:######* .------. .------..------. .:-----: .------.  ------:-------------- ------::-----:.. ------:.-----:.   .------..------. 
       .--:..  . :::.:.---.-:---.#####*- .------. .------..------. .------.::------:  ------:.-----.:------ ------:  :-----: ------:.:------: .------:.:-------. 
        ..:..:::.:::.::....:.::--:####*. .------::------:..------..------:.:::------. ------: .---. :-----: -------::------: ------:  ------: :------..::------: 
       .-::::::---...:--. .-.:-::=###+.  .::::::::::::..  .::::::..::::::.   .::::::. ::::::.  .:.  ::::::: ::::::::::::::.  :::::::  ::::::: ::::::.    .:::::: 
       .--:.   .:--...::. ..::+####*: 
       :--:..   .---:...:.   .:+*+:. 
       .:---:::----.. ....    .:. 
        .:------:..:...  :. 
         .......   ..:.  .. 


                                                                   DIAMBRA™ | Dueling AI Arena
                                                              https://diambra.ai - info@diambra.ai

                                                         DIAMBRA™ is a Trade Mark, © Copyright 2018-2022
                                                                              v. 1.0
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

Environment initialization ...
SHA256 check ok. Correct rom file found.
78081g503.ic655 NOT FOUND (NO GOOD DUMP KNOWN) (tried in doapp coh1002m doapp)
WARNING: the machine might not run correctly.
2188 Completed console init
Num. of Channels = 3
Screen Dim (W x H) = 512 480
(0)Buttons configuration:
(0)  H = But6
(0)  K = But2
(0)  P = But1
(0)Game Continue Val = 0
(0)Show final = 1
(0)1P Environment
(0)Player side = Random
(0)Characters = [Random, Random]
(0)Number of outfits = [2, 2]
done.
Using MultiDiscrete action space
(0)Setting difficulty = 3
(0)Starting game
(0)Player starting side = P2
(0)P2 = Kasumi
(0)Waiting for fight to start
(0)Round done
(0)Moving to next round
```
{{% /tab %}} 
{{% tab name="MacOS" %}}

##### Run Random Agent in Dead Or Alive++ (Headless Mode)

Navigate inside the Examples folder provided with <a href="https://github.com/diambra/diambraArena" target="_blank">DIAMBRA Arena repository</a>, where launcher and Python scripts are located. <span style="color:#333333; font-weight:bolder;">Your <a href="https://diambra.ai/register/" target="_blank">DIAMBRA credentials</a> will be asked at the first execution.</span>

Run the environment as shown below:


```shell
./diambraArena.sh -r "your/roms/local/path" -s diambraArenaGist.py 
```

A successful installation would result in a terminal printout like the following (note that it takes up to a few tens of seconds to complete a round):

```shell
EnvId = TestEnv
Action Spaces = multiDiscrete
Use attack buttons combinations = [True, True]
diambraEnv library successfully loaded

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
      .:-:-**#*#####+***+=-:. 
 ..-++####+#################=+=:. 
:+*#*###########################*-. 
   .-+#############################=. 
      .-*###########################+. 
        .=######++======++*#########*#=. ........ ...     .......     ...........     ........     ........ ............     ............         ........... 
          -*=------:---------=*########- .------..-----:. .------.   .-----------.    --------.   :-------- --------------:. --------------:.    :----------: 
          .:--------:---------:=#######* .------..-------..------.   :-----:-----:.   ---------. .--------- ------:..:-----: ------:..------:   .------:-----. 
        .:------::---::--------:+#######..------. .------..------.  .------.:-----.   ----------.---------- ------:..------. ------:  :-----.  .:-----:.------. 
        :-----::.:---:-----::---:######* .------. .------..------. .:-----: .------.  ------:-------------- ------::-----:.. ------:.-----:.   .------..------. 
       .--:..  . :::.:.---.-:---.#####*- .------. .------..------. .------.::------:  ------:.-----.:------ ------:  :-----: ------:.:------: .------:.:-------. 
        ..:..:::.:::.::....:.::--:####*. .------::------:..------..------:.:::------. ------: .---. :-----: -------::------: ------:  ------: :------..::------: 
       .-::::::---...:--. .-.:-::=###+.  .::::::::::::..  .::::::..::::::.   .::::::. ::::::.  .:.  ::::::: ::::::::::::::.  :::::::  ::::::: ::::::.    .:::::: 
       .--:.   .:--...::. ..::+####*: 
       :--:..   .---:...:.   .:+*+:. 
       .:---:::----.. ....    .:. 
        .:------:..:...  :. 
         .......   ..:.  .. 


                                                                   DIAMBRA™ | Dueling AI Arena
                                                              https://diambra.ai - info@diambra.ai

                                                         DIAMBRA™ is a Trade Mark, © Copyright 2018-2022
                                                                              v. 1.0
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

Environment initialization ...
SHA256 check ok. Correct rom file found.
78081g503.ic655 NOT FOUND (NO GOOD DUMP KNOWN) (tried in doapp coh1002m doapp)
WARNING: the machine might not run correctly.
2188 Completed console init
Num. of Channels = 3
Screen Dim (W x H) = 512 480
(0)Buttons configuration:
(0)  H = But6
(0)  K = But2
(0)  P = But1
(0)Game Continue Val = 0
(0)Show final = 1
(0)1P Environment
(0)Player side = Random
(0)Characters = [Random, Random]
(0)Number of outfits = [2, 2]
done.
Using MultiDiscrete action space
(0)Setting difficulty = 3
(0)Starting game
(0)Player starting side = P2
(0)P2 = Kasumi
(0)Waiting for fight to start
(0)Round done
(0)Moving to next round
```

{{% /tab %}} 
{{< /tabs >}}  




### Advanced

- Install <a href="https://sourceforge.net/projects/vcxsrv/" target="_blank">VcXsrv Windows X Server</a>
- Install socat:
  ```shell
  brew install socat
  ```
- Install <a href="https://www.xquartz.org/releases/XQuartz-2.7.8.html" target="_blank">Xquartz 2.7.8</a>
