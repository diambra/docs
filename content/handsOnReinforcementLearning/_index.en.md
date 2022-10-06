---
title: Hands-on Reinforcement Learning
weight: 70
---

<div style="font-size:1.125rem;">

### Index

- <a href="./#learning-reinforcement-learning">Learning RL</a>
- <a href="./#end-to-end-deep-reinforcement-learning">End-To-End DeepRL</a>

</div>

What is the best path leading a passionate coder to the creation of a trained AI agent capable of effectively playing a videogame? It consists in two steps: learning reinforcement learning and applying it.

[Learning RL](./learningrl) section below deals with how to get started with RL: it presents resources that cover from the basics up to the most advanced details of the latest, best-performing algorithms.

Then, in the [End-to-end DeepRL](./endtoenddeeprl) section, some of the most important tech tools are presented together with a step-by-step guide showing how to successfully train a Deep RL agent in our environments.

### Learning Reinforcement Learning

#### Books

The first suggested step is to learn the basics of Reinforcement Learning. The best option to do so is Sutton & Barto's book "Reinforcement Learning: An Introduction", that can be considered the reference text for the field. An additional option is Packt's "The Reinforcement Learning Workshop" that covers theory but also a good amount of practice, being very hands-on and complemented by a GitHub repo with worked exercises.

<div>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:15%; float:left; width:35.0%">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/books_rlintro.png"/>
   <figcaption align="middle">Reinforcement Learning: An Introduction - Sutton & Barto • <a href="https://mitpress.mit.edu/books/reinforcement-learning-second-edition" target="_blank">Link</a></figcaption>
  </figure>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:auto; margin-left:1%; float:left; width:35.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/books_rlworkshop.png"/>
   <figcaption align="middle">The Reinforcement Learning Workshop - Palmas et al. • <a href="https://www.packtpub.com/product/the-reinforcement-learning-workshop/9781800200456" target="_blank">Link</a></figcaption>
  </figure>
</div>

#### Courses / Video-lectures

An additional useful resource is represented by courses and/or video-lectures. The two listed in this paragraph, in particular, are extremely valuable. The first one, "Berkeley Deep RL Bootcamp", as the title suggests, focuses specifically on Deep RL, and presents a wide overview of the most important, state-of-the-art methods in the field. The second one, "DeepMind Reinforcement Learning Lectures at University College London", deals with RL in general, as Sutton & Barto's does. Both extremely useful and available for free.

<div>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:15%; float:left; width:35.0%">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/courses_deepRlBoot.png"/>
   <figcaption align="middle">Berkeley Deep RL Bootcamp • <a href="https://sites.google.com/view/deep-rl-bootcamp/lectures" target="_blank">Link</a></figcaption>
  </figure>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:auto; margin-left:1%; float:left; width:35.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/courses_deepminducl.png"/>
   <figcaption align="middle">DeepMind Reinforcement Learning Lectures at University College London • <a href="https://www.deepmind.com/learning-resources/reinforcement-learning-lecture-series-2021" target="_blank">Link</a></figcaption>
  </figure>
</div>

#### Research Publications

After having acquired solid fundamentals, as usual in the whole ML domain, one should rely on publications to keep the pace of field advancements. Conference papers, peer-reviewed journal and open access publications are all options to consider.

A good starting point is to read the reference paper for all state-of-the-art algorithms implemented in the most important [RL libraries (see next section)](/deeprltraining/endtoendtraining/#rl-libraries), as found for example <a href="https://stable-baselines3.readthedocs.io/en/master/guide/algos.html" target="_blank">here (SB3)</a> and <a href="https://docs.ray.io/en/latest/rllib/rllib-algorithms.html" target="_blank">here (RAY RLlib)</a>.

<div>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:3%; float:left; width:30.0%">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/papers_arxiv.png"/>
   <figcaption align="middle">Open Access (<a href="https://arxiv.org/search/cs" target="_blank">Arxiv</a>, etc.)</figcaption>
  </figure>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:1%; float:left; width:30.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/papers_conferences.png"/>
   <figcaption align="middle">International Conferences (<a href="https://icml.cc/" target="_blank">ICML</a>, <a href="https://nips.cc/" target="_blank">NeurIPS</a>, <a href="https://iclr.cc/" target="_blank">ICLR</a>, etc.)</figcaption>
  </figure>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:auto; margin-left:1%; float:left; width:30.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/papers_journals.png"/>
   <figcaption align="middle">Peer-reviewed Journals (<a href="https://www.journals.elsevier.com/artificial-intelligence" target="_blank">ELSEVIER</a>, <a href="https://www.springer.com/journal/10458" target="_blank">Springer</a>, etc.)</figcaption>
  </figure>
</div>

#### More

Finally, additional sources of useful information to better understand this field are two films presenting notable milestones achieved by two of the best AI labs in the world, DeepMind and OpenAI. They present their masterpieces AlphaGo/AlphaZero and OpenAI Five, mastering the games of Go and DOTA 2 respectively.

<div>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:1%; margin-left:15%; float:left; width:35.0%">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/film_alphago.png"/>
   <figcaption align="middle">DeepMind - AlphaGo The Movie • <a href="https://www.youtube.com/watch?v=WXuK6gekU1Y" target="_blank">Link</a></figcaption>
  </figure>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:auto; margin-left:1%; float:left; width:35.0%">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/film_artificialGamer.jpg"/>
   <figcaption align="middle">OpenAI - Artificial Gamer Film • <a href="https://youtu.be/J0KPNpro2J8?t=1211" target="_blank">Link</a></figcaption>
  </figure>
</div>

### End-to-End Deep Reinforcement Learning

#### Reinforcement Learning Libraries

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
   <figcaption align="middle">Ray RLlib • <a href="https://docs.ray.io/en/latest/rllib/index.html" target="_blank">Link</a></figcaption>
  </figure>
  <figure style="margin-top:0px;margin-bottom:40px; margin-right:auto; margin-left:1%; float:left; width:30.0%;">
   <img style="margin-bottom: 20px; border-radius: 10px;" src="../../images/deepRlTraining/lib_rlcoach.png"/>
   <figcaption align="middle">Intel RL Coach • <a href="https://intellabs.github.io/coach/" target="_blank">Link</a></figcaption>
  </figure>
</div>

#### DeepRL Agent Training Tutorials

{{% notice tip %}}
All the examples presented in the following sections (plus additional code) showing how to interface DIAMBRA Arena with the major reinforcement learning libraries outthere, can be found in our open source repository <a href="https://github.com/diambra/agents" target="_blank">DIAMBRA Agents</a>.</span>
{{% /notice %}}

<div style="font-size:1.125rem;">

- <a href="./stablebaselines3/">Stable Baselines 3</a>
- <a href="./rayrllib/">Ray RLlib</a>

</div>
