---
title: Closed Beta Guide
disableToc: true
---


```
mkdir agent
cd agent
diambra agent ini .
docker build -t foo/bar .
docker push foo/bar
diambra agent submit --manifest submission.yaml -secret token=<my-secret token>
```

I think this should already print out a url to watch the submission status. If not, fill me an GH issue. The `--secret` stuff is only needed if you reference secrets in `manifest.yaml`.