---
date: 2016-04-09T16:50:16+02:00
title: Games & Specifics
weight: 20
---
<div>
  <figure style="margin-bottom:40px; margin-right:1%; margin-left:auto; float:left; width:15.0%">
   <a href="/envs/games/doapp/"><img style="margin-bottom: 20px; border-radius: 10px;" src="/images/envs/doapp.jpg"/>
   <figcaption align="middle">Dead or Alive ++</figcaption></a>
  </figure>                                                                       
  <figure style="margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="/envs/games/sfiii3n/"><img style="margin-bottom: 20px; border-radius: 10px;" src="/images/envs/sfiii3n.jpg"/>
   <figcaption align="middle">Street Fighter III 3rd Strike</figcaption></a>
  </figure>                                                                       
  <figure style="margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="/envs/games/tektagt/"><img style="margin-bottom: 20px; border-radius: 10px;" src="/images/envs/tektagt.jpg"/>
   <figcaption align="middle">Tekken Tag Tournament</figcaption></a>
  </figure>                                                                       
  <figure style="margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="/envs/games/umk3/"><img style="margin-bottom: 20px; border-radius: 10px;" src="/images/envs/umk3.jpg"/>
   <figcaption align="middle">Ultimate Mortal Kombat 3</figcaption></a>
  </figure>                                                                       
  <figure style="margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="/envs/games/samsh5sp/"><img style="margin-bottom: 20px; border-radius: 10px;" src="/images/envs/samsh5sp.jpg"/>
   <figcaption align="middle">Samurai Showdown 5 Sp</figcaption></a>        
  </figure>                                                                       
  <figure style="margin-bottom:40px; margin-right:auto; margin-left:1%; float:left; width:15.0%;">
   <a href="/envs/games/kof98umh/"><img style="margin-bottom: 20px; border-radius: 10px;" src="/images/envs/kof98umh.jpg"/>
   <figcaption align="middle">The King of Fighers '98 UMH</figcaption></a>
  </figure>                                                                       
</div>

<p style="font-size:35px;  margin-bottom:20px; text-align:center; color: #a5101f;">... many more to come soon.</p>

{{% notice note %}}
Whenever possible, games are released with all hidden/bonus characters unlocked.
{{% /notice %}}

{{% notice info %}}
For every released title, extensive testing has been carried out, being the minimum requirement for a game to be released in beta. After that, the next internal step is training a Deep RL agent to play, and possibly complete it, making sure the 1P mode is playable with no bugs up until game end. This is the condition under which titles are moved from beta to stable status.
{{% /notice %}}
</div>

| <strong><span style="color:#5B5B60;">Title</span></strong> | <strong><span style="color:#5B5B60;">Status</span></strong> | <strong><span style="color:#5B5B60;">Game Id</span></strong>|
|-------------|-------------| ------|                                    
| <a href="/envs/games/doapp/">Dead Or Alive ++</a>                  | Stable[^1]| `doapp`|
| <a href="/envs/games/sfiii3n/">Street Fighter III 3rd Strike</a>     | Stable[^1]| `sfiii3n`|
| <a href="/envs/games/tektagt/">Tekken Tag Tournament</a>             | Stable[^1]| `tektagt`|
| <a href="/envs/games/umk3/">Ultimate Mortal Kombat 3</a>          | Stable[^1]| `umk3`|
| <a href="/envs/games/samsh5sp/">Samurai Showdown 5 Special</a>        | Beta[^2]| `samsh5sp`|
| <a href="/envs/games/kof98umh/">The King of Fighters '98: Ultimate Match Hero</a>    | Beta[^2]| `kof98umh`|

[^1]: Stable = Successfully trained Deep RL Agent in Single Player mode.
[^2]: Beta = Passing all Quality Assurance tests.
