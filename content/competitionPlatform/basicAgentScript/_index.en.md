---
title: Basic Agent Script
weight: 10
math: true
---

The central element of a submission is the agent python script. Its structure is always composed by two main parts, highlighted in the picture below: the preparation step, where the agent and the environment setup is completed, and the interaction loop, where the classical agent-environment interaction happens. 

After a first call to the reset method, you start iterating alternating action selection and environment stepping, resetting the environment when the episode is done. That's it, we take care of the rest. 

There is one thing that is worth noticing: since we want your submission to be the same no matter how many episodes are needed to evaluate it, you need to implement the while loop in a way that it keeps iterating indefinitely (`while True:`) and only exits (the `break` statement) when the value `info["env_done"]` is true. This value is set by us and used to let the agent know that the evaluation has been completed. In this way, the same script can be used to run evaluations made of 3, 5, 10 or whatever number of episodes you want, without changing a single line. 

<figure style="margin-bottom:40px; margin-top:40px; margin-right:auto; margin-left:auto; width: 100%;">
  <img src="/images/agent.jpg" style="margin-top:0px;margin-bottom:20px; margin-right:0px; margin-left:0px;">
  <figcaption align="middle">Structure of an Agent script ready to be submitted</figcaption>
</figure>