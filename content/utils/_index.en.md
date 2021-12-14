---
title: Utils
weight: 40
---

- <a href="/utils/#available-games">Available Games</a>
- <a href="/utils/#check-game-sha256-checksum">Check Game SHA256 Checksum</a>
- <a href="/utils/#environment-spaces-summary">Environment Spaces Summary</a>
- <a href="/utils/#show-gym-observation">Show Gym Observation</a>
- <a href="/utils/#show-wrapped-observation">Show Wrapped Observation</a>

#### Available Games

```python
import diambraArena
diambraArena.availableGames(printOut=True, details=True)
```

#### Check Game SHA256 Checksum

```python
import diambraArena
diambraArena.checkGameSha256(path="path/to/specific/rom/file.zip", gameId=None)
```

#### Environment Spaces Summary
```
from diambraArena.gymUtils import envSpacesSummary
envSpacesSummary(env=environment)
```
#### Show Gym Observation
#### Show Wrapped Observation
