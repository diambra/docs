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
<span style="color:#333333; font-weight:bolder;">In order to use DIAMBRA Arena, you need to <a href="https://diambra.ai/register/" target="_blank">create your account on our website</a>. It requires just a few clicks and is 100% free.</span>
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
  ```shell
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
  ```shell
  docker pull diambra:diambra-arena-base
  ```

- Image with CUDA Support (GPU - CUDA 10.0):
  ```shell
  docker pull diambra:diambra-arena-gpu-cuda10.0
  ```

##### Download Examples

- Download <a href="https://github.com/diambra/DIAMBRAenvironment" target="_blank">TODO MODIFY!DIAMBRA Arena Examples</a>, unzip them and open a terminal inside the newly created folder:

    ```shell
    wget XXX; unzip examples.zip && cd examples
    ```

##### Download Game ROM(s) and Check Validity

- Check available games details running:
    ```shell
    ./diambraArena.sh -l
    ```
  Output example:
  ```shell
   Title: Dead Or Alive ++ - GameId: doapp
     Difficulty levels: Min 1 - Max 4
     SHA256 sum: d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
     Original ROM name: doapp.zip
     Search keywords: ['DEAD OR ALIVE ++ [JAPAN]', 'dead-or-alive-japan', '80781', 'wowroms']
     Characters list: ['Kasumi', 'Zack', 'Hayabusa', 'Bayman', 'Lei-Fang', 'Raidou', 'Gen-Fu', 'Tina', 'Bass', 'Jann-Lee', 'Ayane']

   Title: Street Fighter III - GameId: sfiii3n
     Difficulty levels: Min 1 - Max 8
     SHA256 sum: 7239b5eb005488db22ace477501c574e9420c0ab70aeeb0795dfeb474284d416
     Original ROM name: sfiii3n.zip
     Search keywords: ['STREET FIGHTER III 3RD STRIKE: FIGHT FOR THE FUTUR [JAPAN] (CLONE)', 'street-fighter-iii-3rd-strike-fight-for-the-futur-japan-clone', '106255', 'wowroms']
     Characters list: ['Alex', 'Twelve', 'Hugo', 'Sean', 'Makoto', 'Elena', 'Ibuki', 'Chun-Li', 'Dudley', 'Necro', 'Q', 'Oro', 'Urien', 'Remy', 'Ryu', 'Gouki', 'Yun', 'Yang', 'Ken', 'Gill']

   Title: Tekken Tag Tournament - GameId: tektagt
     Difficulty levels: Min 1 - Max 9
     SHA256 sum: 57be777eae0ee9e1c035a64da4c0e7cb7112259ccebe64e7e97029ac7f01b168
     Original ROM name: tektagtac.zip
     Search keywords: ['TEKKEN TAG TOURNAMENT [ASIA] (CLONE)', 'tekken-tag-tournament-asia-clone', '108661', 'wowroms']
     Notes: Rename the rom from tektagtac.zip to tektagt.zip
     Characters list: ['Xiaoyu', 'Yoshimitsu', 'Nina', 'Law', 'Hwoarang', 'Eddy', 'Paul', 'King', 'Lei', 'Jin', 'Baek', 'Michelle', 'Armorking', 'Gunjack', 'Anna', 'Brian', 'Heihachi', 'Ganryu', 'Julia', 'Jun', 'Kunimitsu', 'Kazuya', 'Bruce', 'Kuma', 'Jack-Z', 'Lee', 'Wang', 'P.Jack', 'Devil', 'True Ogre', 'Ogre', 'Roger', 'Tetsujin', 'Panda', 'Tiger', 'Angel', 'Alex', 'Mokujin', 'Unknown']

   Title: Ultimate Mortal Kombat 3 - GameId: umk3
     Difficulty levels: Min 1 - Max 5
     SHA256 sum: f48216ad82f78cb86e9c07d2507be347f904f4b5ae354a85ae7c34d969d265af
     Original ROM name: umk3r10.zip
     Search keywords: ['ULTIMATE MORTAL KOMBAT 3 (CLONE)', 'ultimate-mortal-kombat-3-clone', '109574', 'wowroms']
     Notes: Rename the rom from umk3r10.zip to umk3.zip
     Characters list: ['Kitana', 'Reptile', 'Kano', 'Sektor', 'Kabal', 'Sonya', 'Mileena', 'Sindel', 'Sheeva', 'Jax', 'Ermac', 'Stryker', 'Shang Tsung', 'Nightwolf', 'Sub-Zero-2', 'Cyrax', 'Liu Kang', 'Jade', 'Sub-Zero', 'Kung Lao', 'Smoke', 'Skorpion', 'Human Smoke', 'Noob Saibot', 'Motaro', 'Shao Kahn']
  ```

- Search ROMs and download them. You can use <span style="color:#333333; font-weight:bolder;">Search Keywords</span> provided by the game list command reported above, there is a list of suggested terms for each game. <span style="color:#333333; font-weight:bolder;">Store all ROMs in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`</span>.

  {{% notice note %}}
  Specific game ROM files are required to make DIAMBRA Arena work. Make sure to check ROMs validity of the downloaded file (next bullet point).
  {{% /notice %}}

- Check ROM(s) validity running:

  ```shell
  ./diambraArena.sh -r "your/roms/local/path" -t romFileName.zip
  ```

  The output for a valid ROM file would look like the following:

  ```shell
  Correct ROM file for Dead Or Alive ++, sha256 = d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
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

  ```shell
  docker pull diambra:diambra-arena-base
  ```
##### Download Examples

- Download <a href="https://github.com/diambra/DIAMBRAenvironment" target="_blank">TODO MODIFY!DIAMBRA Arena Examples</a>, unzip them and open a terminal inside the newly created folder

##### Download Game ROM(s) and Check Validity

- Check available games details running:

    ```shell
    diambraArena.bat -l
    ```
  Output example:

  ```shell
   Title: Dead Or Alive ++ - GameId: doapp
     Difficulty levels: Min 1 - Max 4
     SHA256 sum: d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
     Original ROM name: doapp.zip
     Search keywords: ['DEAD OR ALIVE ++ [JAPAN]', 'dead-or-alive-japan', '80781', 'wowroms']
     Characters list: ['Kasumi', 'Zack', 'Hayabusa', 'Bayman', 'Lei-Fang', 'Raidou', 'Gen-Fu', 'Tina', 'Bass', 'Jann-Lee', 'Ayane']

   Title: Street Fighter III - GameId: sfiii3n
     Difficulty levels: Min 1 - Max 8
     SHA256 sum: 7239b5eb005488db22ace477501c574e9420c0ab70aeeb0795dfeb474284d416
     Original ROM name: sfiii3n.zip
     Search keywords: ['STREET FIGHTER III 3RD STRIKE: FIGHT FOR THE FUTUR [JAPAN] (CLONE)', 'street-fighter-iii-3rd-strike-fight-for-the-futur-japan-clone', '106255', 'wowroms']
     Characters list: ['Alex', 'Twelve', 'Hugo', 'Sean', 'Makoto', 'Elena', 'Ibuki', 'Chun-Li', 'Dudley', 'Necro', 'Q', 'Oro', 'Urien', 'Remy', 'Ryu', 'Gouki', 'Yun', 'Yang', 'Ken', 'Gill']

   Title: Tekken Tag Tournament - GameId: tektagt
     Difficulty levels: Min 1 - Max 9
     SHA256 sum: 57be777eae0ee9e1c035a64da4c0e7cb7112259ccebe64e7e97029ac7f01b168
     Original ROM name: tektagtac.zip
     Search keywords: ['TEKKEN TAG TOURNAMENT [ASIA] (CLONE)', 'tekken-tag-tournament-asia-clone', '108661', 'wowroms']
     Notes: Rename the rom from tektagtac.zip to tektagt.zip
     Characters list: ['Xiaoyu', 'Yoshimitsu', 'Nina', 'Law', 'Hwoarang', 'Eddy', 'Paul', 'King', 'Lei', 'Jin', 'Baek', 'Michelle', 'Armorking', 'Gunjack', 'Anna', 'Brian', 'Heihachi', 'Ganryu', 'Julia', 'Jun', 'Kunimitsu', 'Kazuya', 'Bruce', 'Kuma', 'Jack-Z', 'Lee', 'Wang', 'P.Jack', 'Devil', 'True Ogre', 'Ogre', 'Roger', 'Tetsujin', 'Panda', 'Tiger', 'Angel', 'Alex', 'Mokujin', 'Unknown']

   Title: Ultimate Mortal Kombat 3 - GameId: umk3
     Difficulty levels: Min 1 - Max 5
     SHA256 sum: f48216ad82f78cb86e9c07d2507be347f904f4b5ae354a85ae7c34d969d265af
     Original ROM name: umk3r10.zip
     Search keywords: ['ULTIMATE MORTAL KOMBAT 3 (CLONE)', 'ultimate-mortal-kombat-3-clone', '109574', 'wowroms']
     Notes: Rename the rom from umk3r10.zip to umk3.zip
     Characters list: ['Kitana', 'Reptile', 'Kano', 'Sektor', 'Kabal', 'Sonya', 'Mileena', 'Sindel', 'Sheeva', 'Jax', 'Ermac', 'Stryker', 'Shang Tsung', 'Nightwolf', 'Sub-Zero-2', 'Cyrax', 'Liu Kang', 'Jade', 'Sub-Zero', 'Kung Lao', 'Smoke', 'Skorpion', 'Human Smoke', 'Noob Saibot', 'Motaro', 'Shao Kahn']
  ```

- Search ROMs and download them. You can use <span style="color:#333333; font-weight:bolder;">Search Keywords</span> provided by the game list command reported above, there is a list of suggested terms for each game. <span style="color:#333333; font-weight:bolder;">Store all ROMs in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`</span>.

  {{% notice note %}}
  Specific game ROM files are required to make DIAMBRA Arena work. Make sure to check ROMs validity of the downloaded file (next bullet point).
  {{% /notice %}}

- Check ROM(s) validity running:

  ```shell
  diambraArena.bat "ROMSPATH=your\roms\local\path" "ROMCHECK=<romFile>.zip"
  ```

  The output for a valid ROM file would look like the following:

  ```shell
  Correct ROM file for Dead Or Alive ++, sha256 = d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
  ```

{{% notice tip %}}
To avoid specifying ROMs path for every command you run, you can define a specific environment variable named `DIAMBRAROMSPATH`.<br> 
{{% /notice %}}

{{% /tab %}} 
{{% tab name="MacOS" %}}

##### Download DIAMBRA Arena Docker Images

- Base Image (CPU):

  ```shell
  docker pull diambra:diambra-arena-base
  ```

##### Download Examples

- Download <a href="https://github.com/diambra/DIAMBRAenvironment" target="_blank">TODO MODIFY!DIAMBRA Arena Examples</a>, unzip them and open a terminal inside the newly created folder:

    ```shell
    wget XXX; unzip examples.zip && cd examples
    ```

##### Download Game ROM(s) and Check Validity

- Check available games details running:

    ```shell
    ./diambraArena.sh -l
    ```
  Output example:

  ```shell
   Title: Dead Or Alive ++ - GameId: doapp
     Difficulty levels: Min 1 - Max 4
     SHA256 sum: d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
     Original ROM name: doapp.zip
     Search keywords: ['DEAD OR ALIVE ++ [JAPAN]', 'dead-or-alive-japan', '80781', 'wowroms']
     Characters list: ['Kasumi', 'Zack', 'Hayabusa', 'Bayman', 'Lei-Fang', 'Raidou', 'Gen-Fu', 'Tina', 'Bass', 'Jann-Lee', 'Ayane']

   Title: Street Fighter III - GameId: sfiii3n
     Difficulty levels: Min 1 - Max 8
     SHA256 sum: 7239b5eb005488db22ace477501c574e9420c0ab70aeeb0795dfeb474284d416
     Original ROM name: sfiii3n.zip
     Search keywords: ['STREET FIGHTER III 3RD STRIKE: FIGHT FOR THE FUTUR [JAPAN] (CLONE)', 'street-fighter-iii-3rd-strike-fight-for-the-futur-japan-clone', '106255', 'wowroms']
     Characters list: ['Alex', 'Twelve', 'Hugo', 'Sean', 'Makoto', 'Elena', 'Ibuki', 'Chun-Li', 'Dudley', 'Necro', 'Q', 'Oro', 'Urien', 'Remy', 'Ryu', 'Gouki', 'Yun', 'Yang', 'Ken', 'Gill']

   Title: Tekken Tag Tournament - GameId: tektagt
     Difficulty levels: Min 1 - Max 9
     SHA256 sum: 57be777eae0ee9e1c035a64da4c0e7cb7112259ccebe64e7e97029ac7f01b168
     Original ROM name: tektagtac.zip
     Search keywords: ['TEKKEN TAG TOURNAMENT [ASIA] (CLONE)', 'tekken-tag-tournament-asia-clone', '108661', 'wowroms']
     Notes: Rename the rom from tektagtac.zip to tektagt.zip
     Characters list: ['Xiaoyu', 'Yoshimitsu', 'Nina', 'Law', 'Hwoarang', 'Eddy', 'Paul', 'King', 'Lei', 'Jin', 'Baek', 'Michelle', 'Armorking', 'Gunjack', 'Anna', 'Brian', 'Heihachi', 'Ganryu', 'Julia', 'Jun', 'Kunimitsu', 'Kazuya', 'Bruce', 'Kuma', 'Jack-Z', 'Lee', 'Wang', 'P.Jack', 'Devil', 'True Ogre', 'Ogre', 'Roger', 'Tetsujin', 'Panda', 'Tiger', 'Angel', 'Alex', 'Mokujin', 'Unknown']

   Title: Ultimate Mortal Kombat 3 - GameId: umk3
     Difficulty levels: Min 1 - Max 5
     SHA256 sum: f48216ad82f78cb86e9c07d2507be347f904f4b5ae354a85ae7c34d969d265af
     Original ROM name: umk3r10.zip
     Search keywords: ['ULTIMATE MORTAL KOMBAT 3 (CLONE)', 'ultimate-mortal-kombat-3-clone', '109574', 'wowroms']
     Notes: Rename the rom from umk3r10.zip to umk3.zip
     Characters list: ['Kitana', 'Reptile', 'Kano', 'Sektor', 'Kabal', 'Sonya', 'Mileena', 'Sindel', 'Sheeva', 'Jax', 'Ermac', 'Stryker', 'Shang Tsung', 'Nightwolf', 'Sub-Zero-2', 'Cyrax', 'Liu Kang', 'Jade', 'Sub-Zero', 'Kung Lao', 'Smoke', 'Skorpion', 'Human Smoke', 'Noob Saibot', 'Motaro', 'Shao Kahn']
  ```

- Search ROMs and download them. You can use <span style="color:#333333; font-weight:bolder;">Search Keywords</span> provided by the game list command reported above, there is a list of suggested terms for each game. <span style="color:#333333; font-weight:bolder;">Store all ROMs in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`</span>.

  {{% notice note %}}
  Specific game ROM files are required to make DIAMBRA Arena work. Make sure to check ROMs validity of the downloaded file (next bullet point).
  {{% /notice %}}

- Check ROM(s) validity running:

  ```shell
  ./diambraArena.sh -r "your/roms/local/path" -t romFileName.zip
  ```

  The output for a valid ROM file would look like the following:

  ```shell
  Correct ROM file for Dead Or Alive ++, sha256 = d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
  ```

{{% notice tip %}}
To avoid specifying ROMs path for every command you run, you can define a specific environment variable<br> 
A) in your current shell session with: `export DIAMBRAROMSPATH=your/roms/local/path`<br>
B) permanently, adding `DIAMBRAROMSPATH=your/roms/local/path` to the appropriate shell `.profile` (in `bash`, for example, append the line to `~/.bashrc`)
{{% /notice %}}

{{% /tab %}} 
{{% tab name="Linux (Source)" %}}

##### Install OS Dependencies and Python Packages

- Download or clone <a href="https://github.com/diambra/diambraArena" target="_blank">DIAMBRA Arena Repository</a>
- Open a terminal and navigate inside the repository root
- Execute the installation script to install OS dependencies

  ```shell
  ./setupOS.sh
  ```
- Install the DIAMBRA Arena with PIP

  ```shell
  pip3 install .
  ```
##### Download Game ROM(s) and Check Validity

- Check available games details running:

    ```shell
    python -c "import diambraArena; diambraArena.availableGames(True, True)"
    ```

  Output example:

  ```shell
   Title: Dead Or Alive ++ - GameId: doapp
     Difficulty levels: Min 1 - Max 4
     SHA256 sum: d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
     Original ROM name: doapp.zip
     Search keywords: ['DEAD OR ALIVE ++ [JAPAN]', 'dead-or-alive-japan', '80781', 'wowroms']
     Characters list: ['Kasumi', 'Zack', 'Hayabusa', 'Bayman', 'Lei-Fang', 'Raidou', 'Gen-Fu', 'Tina', 'Bass', 'Jann-Lee', 'Ayane']

   Title: Street Fighter III - GameId: sfiii3n
     Difficulty levels: Min 1 - Max 8
     SHA256 sum: 7239b5eb005488db22ace477501c574e9420c0ab70aeeb0795dfeb474284d416
     Original ROM name: sfiii3n.zip
     Search keywords: ['STREET FIGHTER III 3RD STRIKE: FIGHT FOR THE FUTUR [JAPAN] (CLONE)', 'street-fighter-iii-3rd-strike-fight-for-the-futur-japan-clone', '106255', 'wowroms']
     Characters list: ['Alex', 'Twelve', 'Hugo', 'Sean', 'Makoto', 'Elena', 'Ibuki', 'Chun-Li', 'Dudley', 'Necro', 'Q', 'Oro', 'Urien', 'Remy', 'Ryu', 'Gouki', 'Yun', 'Yang', 'Ken', 'Gill']

   Title: Tekken Tag Tournament - GameId: tektagt
     Difficulty levels: Min 1 - Max 9
     SHA256 sum: 57be777eae0ee9e1c035a64da4c0e7cb7112259ccebe64e7e97029ac7f01b168
     Original ROM name: tektagtac.zip
     Search keywords: ['TEKKEN TAG TOURNAMENT [ASIA] (CLONE)', 'tekken-tag-tournament-asia-clone', '108661', 'wowroms']
     Notes: Rename the rom from tektagtac.zip to tektagt.zip
     Characters list: ['Xiaoyu', 'Yoshimitsu', 'Nina', 'Law', 'Hwoarang', 'Eddy', 'Paul', 'King', 'Lei', 'Jin', 'Baek', 'Michelle', 'Armorking', 'Gunjack', 'Anna', 'Brian', 'Heihachi', 'Ganryu', 'Julia', 'Jun', 'Kunimitsu', 'Kazuya', 'Bruce', 'Kuma', 'Jack-Z', 'Lee', 'Wang', 'P.Jack', 'Devil', 'True Ogre', 'Ogre', 'Roger', 'Tetsujin', 'Panda', 'Tiger', 'Angel', 'Alex', 'Mokujin', 'Unknown']

   Title: Ultimate Mortal Kombat 3 - GameId: umk3
     Difficulty levels: Min 1 - Max 5
     SHA256 sum: f48216ad82f78cb86e9c07d2507be347f904f4b5ae354a85ae7c34d969d265af
     Original ROM name: umk3r10.zip
     Search keywords: ['ULTIMATE MORTAL KOMBAT 3 (CLONE)', 'ultimate-mortal-kombat-3-clone', '109574', 'wowroms']
     Notes: Rename the rom from umk3r10.zip to umk3.zip
     Characters list: ['Kitana', 'Reptile', 'Kano', 'Sektor', 'Kabal', 'Sonya', 'Mileena', 'Sindel', 'Sheeva', 'Jax', 'Ermac', 'Stryker', 'Shang Tsung', 'Nightwolf', 'Sub-Zero-2', 'Cyrax', 'Liu Kang', 'Jade', 'Sub-Zero', 'Kung Lao', 'Smoke', 'Skorpion', 'Human Smoke', 'Noob Saibot', 'Motaro', 'Shao Kahn']
  ```

- Search ROMs and download them. You can use <span style="color:#333333; font-weight:bolder;">Search Keywords</span> provided by the game list command reported above, there is a list of suggested terms for each game. <span style="color:#333333; font-weight:bolder;">Store all ROMs in the same folder, whose absolute path will be referred in the following as `your/roms/local/path`</span>.

  {{% notice note %}}
  Specific game ROM files are required to make DIAMBRA Arena work. Make sure to check ROMs validity of the downloaded file (next bullet point).
  {{% /notice %}}

- Check ROM(s) validity running:

  ```shell
  python -c "import diambraArena, os; diambraArena.checkGameSha256(os.path.join('your/roms/local/path', 'romFileName.zip'))"
  ```

  The output for a valid ROM file would look like the following:

  ```shell
  Correct ROM file for Dead Or Alive ++, sha256 = d95855c7d8596a90f0b8ca15725686567d767a9a3f93a8896b489a160e705c4e
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

##### CUDA Installation Test

```shell
./diambraArena.sh -c "cat /proc/driver/nvidia/version; nvcc -V" -d GPU
```
A successful installation would result in a terminal printout like the following:

```shell
NVRM version: NVIDIA UNIX x86_64 Kernel Module  440.95.01  Thu May 28 07:03:08 UTC 2020
GCC version:  gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04) 
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sat_Aug_25_21:08:01_CDT_2018
Cuda compilation tools, release 10.0, V10.0.130
```

{{% /tab %}}
{{% tab name="Windows" %}}

##### Run Random Agent in Dead Or Alive++ (Headless Mode)

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
{{% tab name="Linux (Source)" %}}

##### Run Random Agent in Dead Or Alive++

Navigate inside the Examples folder provided by DIAMBRA Arena Repo and execute the basic the Python script as follows:

```shell
python diambraArenaGist.py --romsPath "your/roms/local/path" 
```

A successful installation would result in the game screen rendered inside a new window and a terminal printout like the following:

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
