---
date: 2016-04-09T16:50:16+02:00
title: Dead Or Alive ++
weight: 10
---

Output example: 
### Observation Space
<figure style="margin-bottom:0px; margin-top:0px; margin-right:auto; margin-left:auto;">
  <a href="/images/envs/doappData.png" target="_blank"><img src="/images/envs/doappData.png" style="margin-bottom:20px;"></a>
  <figcaption align="middle">External aerodynamics showing pressure-colored flow lines and turbulent wake</figcaption>
</figure>

- Global:
  - *stage (1P mode only)* | Discrete ordered: from 0 to 8 (final stage number)
- Player-specific:
  - *OwnSide/OppSide* | Discrete binary:
    - 0: Player on the left of the stage
    - 1: Player on the right of the stage
  - *OwnWins/OppWins* | Discrete ordered: from 0 to 2 (maximum number of rounds to win a stage/match)
  - *OwnChar/OppChar* | Discrete categorical: 
    - 0: Kasumi
    - 1: Zack
    - 2: Hayabusa
    - 3: Bayman
    - 4: Lei-Fang
    - 5: Raidou
    - 6: Gen-Fu
    - 7: Tina
    - 8: Bass
    - 9: Jann-Lee
    - 10: Ayane
  - *OwnHealth/OppHealth* | Discrete ordered: from 0 to 208 (Health value at the beginning of the round)

### Action Space
