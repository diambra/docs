---
title: "Home"
---

# DIAMBRA Arena Docs

<figure style="margin-bottom:40px; margin-top:0px; margin-right:auto; margin-left:auto; width: 100%;">
  <img src="./images/home/envCover.png" style="margin-top:0px;">
</figure>   

DIAMBRA Arena is a software package featuring a collection of <span style="color:#333333; font-weight:bolder;">high-quality environments for Reinforcement Learning research and experimentation</span>. It acts as an interface towards popular arcade emulated video games, offering a <span style="color:#333333; font-weight:bolder;">Python API fully compliant with OpenAI Gym standard</span>, that makes its adoption smooth and straightforward. 

It <span style="color:#333333; font-weight:bolder;">supports all major Operating Systems: Linux, Windows and MacOS</span>, most of them via Docker, with a step by step installation guide available in this manual. It is <span style="color:#333333; font-weight:bolder;">completely free to use</span>, the user only needs to register on the official website. 


In addition, its <a href="https://github.com/diambra/diambraArena" target="_blank">GitHub repository</a> provides a <span style="color:#333333; font-weight:bolder;">collection of examples</span> covering main use cases of interest <span style="color:#333333; font-weight:bolder;">that can be run in just a few steps</span>.

<figure style="margin-bottom:40px; margin-top:0px; margin-right:auto; margin-left:auto; width: 80%;">
  <img src="./images/envs/basicUsage.png" style="margin-bottom:20px;">           
  <figcaption align="middle">Agent-Environment Interaction Scheme</figcaption>
</figure>   

#### Main Features

All environments are episodic Reinforcement Learning tasks, with discrete actions (gamepad buttons) and observations composed by screen pixels plus additional numerical data (RAM values like characters health bars or characters stage side). 

They all  <span style="color:#333333; font-weight:bolder;">support both single player (1P) as well as two players (2P) mode</span>, making them the perfect resource to explore all the following Reinforcement Learning subfields: 

<div style="margin-bottom:0px;">
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:110px; max-width:15%; min-height:120px;">
   <img style="margin-top:0px; margin-bottom:10px; border-radius: 10px;" src="./images/home/AIvsCOM.png"/>
   <figcaption align="middle">Standard RL</figcaption>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:110px; max-width:15.0%; min-height:120px;">
   <img style="margin-top:0px; margin-bottom:10px; border-radius: 10px;" src="./images/home/AIvsAI.png"/>
   <figcaption align="middle">Competitive Multi-Agent</figcaption>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:110px; max-width:15.0%; min-height:120px;">
   <img style="margin-top:0px; margin-bottom:10px; border-radius: 10px;" src="./images/home/AIvsHUM.png"/>
   <figcaption align="middle">Competitive Human-Agent</figcaption>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:110px; max-width:15.0%; min-height:120px;">
   <img style="margin-top:0px; margin-bottom:10px; border-radius: 10px;" src="./images/home/SP.png"/>
   <figcaption align="middle">Self-Play</figcaption>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:110px; max-width:15.0%; min-height:120px;">
   <img style="margin-top:0px; margin-bottom:10px; border-radius: 10px;" src="./images/home/IL.png"/>
   <figcaption align="middle">Imitation Learning</figcaption>
  </figure>
  <figure style="padding:2px; margin-right:auto; margin-left:auto; float:left; min-width:110px; max-width:15.0%; min-height:120px;">
   <img style="margin-top:0px; margin-bottom:10px; border-radius: 10px;" src="./images/home/HITL.png"/>
   <figcaption align="middle">Human-in-the-Loop</figcaption>
  </figure> 
</div>

#### Available Games

Interfaced games have been selected among the most popular fighting retro-games. While sharing the same fundamental mechanics, they provide slightly different challenges, with specific features such as different type and number of characters, how to perform combos, health bars recharging, etc.  

Whenever possible, games are released with all hidden/bonus characters unlocked. 

Additional details can be found in their <a href="./envs/games/">dedicated section</a>.

<div>                                                                           
  <figure style="margin-right:1%; margin-left:auto; float:left; width:15.0%">
   <a href="./envs/games/"><img style="margin-top:0px; margin-bottom:30px; border-radius: 10px;" src="./images/envs/doapp.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="./envs/games/"><img style="margin-top:0px; margin-bottom:30px; border-radius: 10px;" src="./images/envs/sfiii3n.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="./envs/games/"><img style="margin-top:0px; margin-bottom:30px; border-radius: 10px;" src="./images/envs/tektagt.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="./envs/games/"><img style="margin-top:0px; margin-bottom:30px; border-radius: 10px;" src="./images/envs/umk3.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-right:1%; margin-left:1%; float:left; width:15.0%;">
   <a href="./envs/games/"><img style="margin-top:0px; margin-bottom:30px; border-radius: 10px;" src="./images/envs/samsh5sp.jpg"/></a>
  </figure>                                                                     
  <figure style="margin-right:auto; margin-left:1%; float:left; width:15.0%;">
   <a href="./envs/games/"><img style="margin-top:0px; margin-bottom:30px; border-radius: 10px;" src="./images/envs/kof98umh.jpg"/></a>
  </figure>                                                                     
</div>                                                                          
       

### Docs Structure

<div style="font-size:1.125rem;">

- <a href="./installation/">Installation</a>              
- <a href="./gettingstarted/">Getting Started</a>              
    - <a href="./gettingstarted/examples/">Examples</a>              
- <a href="./envs/">Environments</a>              
    - <a href="./envs/games/">Games & Specifics</a>              
- <a href="./wrappers/">Wrappers</a>              
- <a href="./utils/">Utils</a>              
- <a href="./imitationlearning/">Imitation Learning</a>              

</div>

### Support, Feature Requests & Bugs Reports

To receive support, use the dedicated channel in our <a href="https://discord.gg/tFDS2UN5sv" target="_blank">Discord Server</a>.

To request features or report bugs, use the <a href="https://github.com/diambra/diambraArena/issues" target="_blank">GitHub Issue Tracker</a>.

### References

- Official Website: <a href="https://diambra.ai" target="_blank">https://diambra.ai</a>
- GitHub Repository: <a href="https://github.com/diambra/arena" target="_blank">https://github.com/diambra/arena</a>
- Docker Repository: <a href="https://hub.docker.com/r/diambra/diambra-arena" target="_blank">https://hub.docker.com/r/diambra/diambra-arena</a>
- Linkedin Page: <a href="https://www.linkedin.com/company/diambra" target="_blank">https://www.linkedin.com/company/diambra</a>
- Discord Server: <a href="https://discord.gg/tFDS2UN5sv" target="_blank">https://discord.gg/tFDS2UN5sv</a>
- Twitch Channel: <a href="https://www.twitch.tv/diambra_ai" target="_blank">https://www.twitch.tv/diambra_ai</a>
- YouTube Channel: <a href="https://www.youtube.com/c/diambra_ai" target="_blank">https://www.youtube.com/c/diambra_ai</a>

### Citation

```LaTex
  @misc{diambra2022
    author = {DIAMBRA Team},
    title = {DIAMBRA Arena: built with OpenAI Gym Python interface, easy to use, transforms popular video games into Reinforcement Learning environments.},
    year = {2022},
    howpublished = {\url{https://github.com/diambra/diambraArena}},
  }
```

### Terms of Use

DIAMBRA Arena software package is subject to our <a href="https://diambra.ai/terms" target="_blank">Terms of Use</a>. By using it, you accept them in full.


###### DIAMBRA™ is a Trade Mark, © Copyright 2018-2022. All Rights Reserved.
