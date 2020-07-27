---
layout: page
title: Quickstart
---

# Quickstart

PipelineBot is a GitHub app, to get started with pipelines we need a
repository and to install the PipelineBot app onto that repo. The below guide
will walk you through getting an example repository setup.

### 1. [Fork the example repository][example]{:target="_blank"}
The example repository demonstrates a simple pipeline that contains 4 stages.
For more information, checkout the [guide][guide]{:target="_blank"}.

### 2. [Install PipelineBot App][app] from GitHub marketplace
Make sure you select the organisation that contains the repository your just forked.

<!--TODO: add images to demonstrate the steps to install the GitHub App -->

### 3. Push a commit to watch the pipeline kick off
Make some changes (e.g. update the README.md), commit and push it. This will trigger
the whole pipeline.

<!--TODO:
1. add a link to update the README.md;
2. explain the difference between triggering a workflow and trigger a pipeline
-->

### 4. Manually trigger the last stage `Production`
PipelineBot will automatically trigger the following two stages - `UAT` and `Staging`.
However, in some cases (more often than not), you want the last step `Production` to
be triggered manually. Once the previous three stages finish, you can pick one version
to trigger the production deployment.

## Next

Once you have a working example read through the [guide](/docs/guide/) for more
information on how to setup complex workflows.

[guide]: /docs/guide
[how]: /docs/how-it-works/
[app]: https://github.com/marketplace/pipelinebotapp
