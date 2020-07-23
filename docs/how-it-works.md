---
layout: page
title: How it works
---

# How it works

PipelineBot is built around [GitHub Actions][1]. It builds pipelines based on GitHub workflow.
It allows you to join your workflows as pipelines with auto or manual trigger. It's an event
driven decoupled way to deploy your code. Pipeline is responsible for
figuring out when to trigger a task. When conditions are met it triggers
a task to the environment that you've specified in the [configuration file][2].
Once the task is triggered, your script defined in the workflow will execute as designed.

The diagram below details what a deployment looks like:
![how-it-works](/assets/images/docs-how-it-works.png)

The benefit to this architecture is that your workflows are separate from
the platform that schedule them. If you change platforms or use multiple
platforms two schedule your workflows you can still enforce the same continuous delivery
processes.

This architecture is well suited for [GitHub Actions][1] which is the GitHub
supported system for running your code in response to GitHub events. It means
that setting up PipelineBot is much simpler since you don't also have to manage
infrastructure to listen to events. The next sections go through guides on how
to implement auto trigger and manual trigger.

[1]: https://github.com/features/actions/
[2]: /docs/configuration/

## Auto trigger

PipelineBot can be looked as a wrapper of GitHub Actions. Actions
run on GitHub infrastructure in response to GitHub events. The basic format for
setting up a GitHub action looks like this:

```yaml
# .github/workflows/build.yml
name: 'Build'
on: [push]

jobs:
  build:
    runs-on: 'ubuntu-latest'
    steps: []
    # Steps to execute your build.
```

```yaml
# .github/workflows/test.yml
name: 'Test'
on: workfolow_dispatch

jobs:
  build:
    runs-on: 'ubuntu-latest'
    steps: []
    # Steps to execute your test.
```

The following configuration sets up a pipeline that triggers the *Test* workflow
automatically when *Build* finishes.

```yaml
version: 0.3.0-beta
stages:
  - Build
  - Test
```

The *Build* workflow that will run on `push` events triggered
in your specific repository. When the *Build* workflow finishes, 
PipelineBot triggers the *Test* events via dispatching a `workflow_dispatch` event.

## Manual trigger

Let's say we have a third stage `Deploy` that you would like to trigger it manually.
The corresponding workflow looks like this:

```yaml
name: 'Deploy'
on: workflow_dispatch

jobs:
  build:
    runs-on: 'ubuntu-latest'
    steps: []
    # Steps to execute your deployment.
``` 

In the pipeline configuration file, you need to add a new stage and mark the trigger
as manual.

```yaml
version: 0.3.0-beta
stages:
  - Build
  - Test
  - Deploy: manual
```
