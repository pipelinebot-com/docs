---
layout: page
title: Pipelines
---

Pipelines are the top-level component of continuous integration, deployment and delivery.

Pipelines comprise:
* `stage` that defines *when* to run the tasks. For example, stages that deploy packages to UAT
after stages the run the tests.
* `tasks` that defines *what* to do. For example, tasks that compile the code and run unit tests.

Tasks are executed by [GitHub Actions][1]. Multiple tasks in the same stage are executed in parallel.
There are some limits on GitHub Actions usage. Checkout [usage limits][3] for details.

> A Job is a set of steps that execute on the same  runner. Jobs can run at the same time in parallel
or run sequentially depending on the status of a previous job.

If *all* tasks in a stage succeed, the pipeline moves on to the next stage.

If *any* tasks in a stage fails, the next stage is not (usually) executed and the pipeline ends early.

In general, pipelines are executed automatically by default and require no intervention.
 However there are also times you may want to manually interact with a pipeline.
 PipelineBot allows you to configure a stage to be *manual*. See [manual trigger][4] for instructions.
 
 A typical pipeline might consist of four stages, executed in the following order:
 * `CI` with a task that runs build, unit test and integration test.
 * `UAT` with a task that deploys the package that is generated and tested in the `CI` stage to the
 UAT environment, and run UAT tests.
 * `Staging` with a task that deploys the package to the `Staging` environment.
 * `Production` with a task that deploys the package to the `Production` environment.
  

# View pipelines

You can find the current and historical pipelines runs under your repository's pipelines page.

# Run a pipeline manually

Stages in a pipeline can be manually executed. You might do this if some of the steps needs
approval.

You can do this straight from the pipeline graph. Just click the play button to execute that particular
stage.

For example, your pipeline might start automatically, but it requires manual action to deploy to
production. In the example below, the production stage has a job with a manual action.

# Common issues and recommendations

## Flaky jobs

It happens that some of the jobs are flaky. If you put the flaky jobs in a workflow with other stable
jobs, if the flaky job fails, the whole workflow fails.

With PipelineBot, you can put the flaky jobs in a separate workflow. If they fail, you can manually
rerun this workflow. You can even split them into a few workflows and rerun only the failed one(s).

> Consider fix the flaky step and improve the stability of the pipeline.

You may find that you do not need [the ability to rerun a single job in a workflow][5] anymore :)



[1]: https://github.com/features/actions/
[2]: /docs/configuration/
[3]: https://docs.github.com/en/actions/getting-started-with-github-actions/about-github-actions#usage-limits
[4]: http://localhost:3000/docs/how-it-works/#manual-trigger
[5]: https://github.community/t/ability-to-rerun-just-a-single-job-in-a-workflow/17234?u=mrcoder
