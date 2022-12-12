---
date: 2016-04-09T16:50:16+02:00
title: Games & Specifics
weight: 20
---

<div style="display:block; width:100%;">
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:90px; max-width:15.0%; min-height:230px;">
   <a href="./doapp/"><img style="margin-top:0px; margin-bottom: 20px; border-radius: 10px;" src="../../images/envs/doapp.jpg"/>
   <figcaption align="middle">Dead or Alive ++</figcaption></a>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:90px; max-width:15.0%; min-height:230px;">
   <a href="./sfiii3n/"><img style="margin-top:0px; margin-bottom: 20px; border-radius: 10px;" src="../../images/envs/sfiii3n.jpg"/>
   <figcaption align="middle">Street Fighter III 3rd Strike</figcaption></a>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:90px; max-width:15.0%; min-height:230px;">
   <a href="./tektagt/"><img style="margin-top:0px; margin-bottom: 20px; border-radius: 10px;" src="../../images/envs/tektagt.jpg"/>
   <figcaption align="middle">Tekken Tag Tournament</figcaption></a>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:90px; max-width:15.0%; min-height:230px;">
   <a href="./umk3/"><img style="margin-top:0px; margin-bottom: 20px; border-radius: 10px;" src="../../images/envs/umk3.jpg"/>
   <figcaption align="middle">Ultimate Mortal Kombat 3</figcaption></a>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:90px; max-width:15.0%; min-height:230px;">
   <a href="./samsh5sp/"><img style="margin-top:0px; margin-bottom: 20px; border-radius: 10px;" src="../../images/envs/samsh5sp.jpg"/>
   <figcaption align="middle">Samurai Showdown 5 Sp</figcaption></a>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:90px; max-width:15.0%; min-height:230px;">
   <a href="./kof98umh/"><img style="margin-top:0px; margin-bottom: 20px; border-radius: 10px;" src="../../images/envs/kof98umh.jpg"/>
   <figcaption align="middle">The King of Fighers '98 UMH</figcaption></a>
  </figure>
</div>

<div>
<p style="font-size:35px;  margin-bottom:20px; text-align:center; color: #a5101f; clear:both;">... many more to come soon.</p>

### Game Specific Info

Game specific details provide useful information about each title. They are reported in every game-dedicated page, and summarized in the table below.

| <strong><span style="color:#5B5B60;">Parameter</span></strong>                                                           | <strong><span style="color:#5B5B60;">Description</span></strong>                        |
| ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| <strong><span style="color:#5B5B60;">Game ID</span></strong>                                                             | String identifying the game                                                             |
| <strong><span style="color:#5B5B60;">Original ROM Name</span></strong>                                                   | Name of the original game ROM to be downloaded (if renaming is needed, it is indicated) |
| <strong><span style="color:#5B5B60;">SHA256 Checksum</span></strong>                                                     | ROM file checksum used to validate it                                                   |
| <strong><span style="color:#5B5B60;">Search Keywords</span></strong>                                                     | List of keywords that can be used to find the correct ROM file                          |
| <strong><span style="color:#5B5B60;">Game Resolution (H X W X C)</span></strong>                                         | Game frame resolution                                                                   |
| <strong><span style="color:#5B5B60;">Number of Moves and Attack Actions<br>(Without Buttons Combination)</span></strong> | Number of moves and attack actions and their description                                |
| <strong><span style="color:#5B5B60;">Max Difficulty</span></strong>                                                      | Maximum difficulty level available                                                      |
| <strong><span style="color:#5B5B60;">Number of Characters (Selectable)</span></strong>                                   | Number of characters featured in the game, and those that can actually be selected      |
| <strong><span style="color:#5B5B60;">Max Number of Outfits</span></strong>                                               | Maximum number of different outfits available per each character                        |
| <strong><span style="color:#5B5B60;">Max Stage</span></strong>                                                           | Maximum number of stages for the single player mode                                     |

{{% notice note %}}
Whenever possible, games are released with all hidden/bonus characters unlocked.
{{% /notice %}}

{{% notice info %}}
For every released title, extensive testing has been carried out, being the minimum requirement for a game to be released in beta. After that, the next internal step is training a Deep RL agent to play, and possibly complete it, making sure the 1P mode is playable with no bugs up until game end. This is the condition under which titles are moved from beta to stable status.
{{% /notice %}}

</div>

| <strong><span style="color:#5B5B60;">Title</span></strong>              | <strong><span style="color:#5B5B60;">Status</span></strong> | <strong><span style="color:#5B5B60;">Game Id</span></strong> |
| ----------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------ |
| <a href="./doapp/">Dead Or Alive ++</a>                                 | Stable[^1]                                                  | `doapp`                                                      |
| <a href="./sfiii3n/">Street Fighter III 3rd Strike</a>                  | Stable[^1]                                                  | `sfiii3n`                                                    |
| <a href="./tektagt/">Tekken Tag Tournament</a>                          | Stable[^1]                                                  | `tektagt`                                                    |
| <a href="./umk3/">Ultimate Mortal Kombat 3</a>                          | Stable[^1]                                                  | `umk3`                                                       |
| <a href="./samsh5sp/">Samurai Showdown 5 Special</a>                    | Beta[^2]                                                    | `samsh5sp`                                                   |
| <a href="./kof98umh/">The King of Fighters '98: Ultimate Match Hero</a> | Beta[^2]                                                    | `kof98umh`                                                   |

[^1]: Stable = Successfully trained Deep RL agent in single player mode.
[^2]: Beta = Passing all quality assurance tests.
