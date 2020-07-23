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

GitHub actions are one of the simplest ways to handle your deployments. Actions
run on GitHub infrastructure in response to GitHub events. The basic format for
setting up a GitHub action to handle a deployment looks like this:

```yaml
# .github/workflows/deploy.yml
name: 'Deploy'
on: ['deployment']

jobs:
  deployment:
    runs-on: 'ubuntu-latest'
    steps: []
    # Steps to execute your deployment.
```

The above instantiates a workflow that will run on `deployment` events triggered
in your specific repository. Deliverybot triggers these events and your code
does the work of shipping the actual deployments onto your infrastructure.

If you are building your own actions, one important thing that you may notice
is that the deployment is always sitting in 'pending' or 'waiting'. For
GitHub and Deliverybot to be able to track your deployment you need to send back
`deployment_status` events. Below is a template for doing just that, we have
actions that run before and after your main deployment block which set the
correct status:

```yaml
# .github/workflows/deploy.yml
name: 'Deploy'
on: ['deployment']

jobs:
  deployment:
    runs-on: 'ubuntu-latest'
    steps:
    - name: 'Checkout'
      uses: 'actions/checkout@v1'

    - name: 'Deployment pending'
      uses: 'deliverybot/deployment-status@master'
      with:
        state: 'pending'
        token: '${{ github.token }}'

    - name: 'Deploy ${{ github.event.deployment.environment }}'
      run: |
        echo 'YOUR CODE HERE'

    - name: 'Deployment success'
      if: success()
      uses: 'deliverybot/deployment-status@master'
      with:
        state: 'success'
        token: '${{ github.token }}'

    - name: 'Deployment failure'
      if: failure()
      uses: 'deliverybot/deployment-status@master'
      with:
        state: 'failure'
        token: '${{ github.token }}'
```

## Deliverying deployments with external tooling

Since it's just listening to GitHub events to execute a deployment you can use
a wide variety of tooling to actually ship deployments. The following guide
includes details on how to write code to handle deployments:

<https://developer.github.com/v3/guides/delivering-deployments/>
