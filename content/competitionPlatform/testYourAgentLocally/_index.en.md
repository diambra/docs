---
title: Test Your Agent Locally
weight: 80
math: true
---

If you want to test your agent locally before submitting it for evaluation on the platform, you can use the specific feature provided by our command line interface. The pattern of the command is the very same used for submission, except that instead of the `submit` option you will use `test`. 

It can be used to make sure the agent behaves as expected, and to debug it in case it fails, without waiting for the online evaluation pipeline.

It works with both plain docker images as well as submission manifests with privately hosted files and secret tokens, using respectively, the following commands:

```shell
diambra agent test <docker image>
```
or
```shell
diambra agent test  --submission.secret token=<my-secret token> --submission.manifest submission.yaml
```