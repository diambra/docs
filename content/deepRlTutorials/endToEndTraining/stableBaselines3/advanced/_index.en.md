---
title: Advanced
weight: 20
---

<div style="font-size:1.125rem;">

### Index

- <a href="./#rl-libraries">RL Libraries</a>
- <a href="./#deeprl-agent-training">DeepRL Agent Training</a>

</div>

This page aims at guiding the user, who is expected to be at least familiar with the basic concepts of RL ([see Learning RL page](/deeprltraining/learningrl/)), in a step-by-step process that will make him able to train a state-of-the-art DeepRL agent capable of obtaining good performances in our environments.

### RL Libraries

If one wants to rely on already implemented RL algorithms, focusing his efforts on higher level aspects such as policy network architecture, features selection, hyper-parameters tuning, and so on, the best choice is to leverage state-of-the-art RL libraries as the ones shown below. There are many different options, here we list those that, in our experience, are recognized as the leaders in the field, and have been proven to achieve good performances in DIAMBRA Arena environments.

There are multiple advantages related to the use of these libraries, to name a few: they provide high quality RL algorithms, efficiently implemented and continuously tested, they allow to natively parallelize environment execution, and in some cases they even support distributed training using multiple GPUs in a single workstation or even in cluster contexts.

We will provide guidance and examples using some of the options listed down here.

<div>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:3%; float:left; width:30.0%">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/lib_sb3.png"/>
   <figcaption align="middle">Stable Baselines 3 • <a href="https://stable-baselines3.readthedocs.io/en/master/" target="_blank">Link</a></figcaption>
  </figure>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:30.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/lib_rayrllib.png"/>
   <figcaption align="middle">Ray RLLib • <a href="https://docs.ray.io/en/latest/rllib/index.html" target="_blank">Link</a></figcaption>
  </figure>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:auto; margin-left:1%; float:left; width:30.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/lib_rlcoach.png"/>
   <figcaption align="middle">Intel RL Coach • <a href="https://intellabs.github.io/coach/" target="_blank">Link</a></figcaption>
  </figure>
</div>

### DeepRL Agent Training

Stable-Baselines3
