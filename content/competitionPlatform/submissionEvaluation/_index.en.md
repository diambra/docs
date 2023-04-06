---
title: Submission Evaluation
weight: 20
math: true
---

Each time you submit an agent, it is run for one episode to be evaluated. Every submission will generate a score, used for leaderboard positioning, and will unlock achievements. 

The score is a function of both the total cumulative reward and the submission difficulty you selected at submission time, which can be either "Easy", "Medium" or "Hard". Every game has a different difficulty level scale, so a specific mapping is applied and is represented by the following table:

| <strong><span style="color:#5B5B60;">Game</span></strong> | <strong><span style="color:#5B5B60;">Easy</span></strong> | <strong><span style="color:#5B5B60;">Medium</span></strong> | <strong><span style="color:#5B5B60;">Hard</span></strong> |
| ------------------------------------------------------------ | :----------------------------------------------------------------: | :----------------------------------------------------------------: | :----------------------------------------------------------------: |
| Dead Or Alive ++                                               | 2 | 3 | 4 |
| Street Fighter III                                             | 4 | 6 | 8 |
| Tekken Tag Tournament                                          | 5 | 7 | 9 |
| Ultimate Mortal Kombat 3                                       | 3 | 4 | 5 |
| Samurai Showdown 5                                             | 4 | 6 | 8 |
| King of Fighters '98                                           | 4 | 6 | 8 |

The relation that links score with total cumulative reward and difficulty is shown in the picture below. When "Easy" is selected, the score is exactly equal to the total cumulative reward. When "Medium" (or "Hard") is selected, the score is obtained multiplying the total cumulative reward by a weighting value that varies linearly with the total cumulative reward obtained. It is equal to 1 if you obtain the lowest possible total cumulative reward (i.e. same score as if "Easy" was selected), and it is equal to the ratio between the game difficulty level for "Medium" (or "Hard") and the game difficulty level for "Easy" if you obtain the highest possible total cumulative reward.

So, for example, for Dead or Alive ++, the weighting values for "Medium" and "Hard" vary linearly between

$$
\begin{equation}
\begin{gathered}
k_M = \left[1.0,  \frac{3}{2} \right] = \left[1.0,  1.5 \right] \\\\
k_H = \left[1.0,  \frac{4}{2} \right] = \left[1.0,  2.0 \right]
\end{gathered}
\end{equation}
$$

<figure style="margin-bottom:40px; margin-top:40px; margin-right:auto; margin-left:auto; width: 100%;">
  <img src="/images/score_chart.jpg" style="margin-top:0px;margin-bottom:20px; margin-right:0px; margin-left:0px;">
  <figcaption align="middle">Scoring as a function of Total Cumulative Reward and Submission Difficulty</figcaption>
</figure>