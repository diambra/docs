---
title: Arguments and Commands
weight: 60
math: true
---

In case you want to specify command line arguments and/or overriding the image entrypoint at submission time, you can leverage the command line interface. Here are the different use cases covered:

##### Add arguments to a given docker image

```shell
diambra agent submit <docker image> arg1 arg2
```
the correspondent submission manifest would use a new `args` keyword as follows:
```yaml
---
image: <docker image>
mode: AIvsCOM
version: v2.1
difficulty: easy
args:
- arg1
- arg2
```
##### Add arguments to a given submission manifest

```shell
diambra agent submit --submission.manifest manifest.yaml arg1 arg2 arg3
```
the resulting submission manifest sent to the platform would be
```yaml
---
image: diambra/agent-random-1:2.1
mode: AIvsCOM
version: v2.1
difficulty: easy
args:
- arg1
- arg2
- arg3
```

##### Override entrypoint of a given image

```shell
diambra agent submit --submission.set-command <docker image> command arg1 arg2
```
the correspondent submission manifest would be:
```yaml
---
image: <docker image>
mode: AIvsCOM
version: v2.1
difficulty: easy
command:
- command
- arg1
- arg2
```

##### Override entrypoint of an image specified in a given submission manifest

```shell
diambra agent submit --submission.set-command --submission.manifest manifest.yaml command arg1 arg2
```
the resulting submission manifest sent to the platform would be
```yaml
---
image: diambra/agent-random-1:2.1
mode: AIvsCOM
version: v2.1
difficulty: easy
command:
- command
- arg1
- arg2
```