---
title: Stable Baselines 3
weight: 10
---

This page aims at guiding the user, who is expected to be at least familiar with the basic concepts of RL ([see Learning RL page](/deeprltraining/learningrl/)), in a step-by-step process that will make him able to train a state-of-the-art DeepRL agent capable of obtaining good performances in our environments.

### Getting Ready

```shell
conda create -n diambra-arena-sb3 python=3.7
```

```shell
pip install stable-baselines3[extra]
pip install gym[all]
pip install diambra-arena
```

<a href="https://stable-baselines3.readthedocs.io/en/master/guide/install.html" target="_blank">Stable-Baselines3 Installation Docs</a>

If one wants to rely on already implemented RL algorithms, focusing his efforts on higher level aspects such as policy network architecture, features selection, hyper-parameters tuning, and so on, the best choice is to leverage state-of-the-art RL libraries as the ones shown below. There are many different options, here we list those that, in our experience, are recognized as the leaders in the field, and have been proven to achieve good performances in DIAMBRA Arena environments.

There are multiple advantages related to the use of these libraries, to name a few: they provide high quality RL algorithms, efficiently implemented and continuously tested, they allow to natively parallelize environment execution, and in some cases they even support distributed training using multiple GPUs in a single workstation or even in cluster contexts.

We will provide guidance and examples using some of the options listed down here.

### DeepRL Agent Training

<div style="font-size:1.125rem;">

- <a href="./basic/">Basic</a>
- <a href="./advanced/">Advanced</a>

</div>
