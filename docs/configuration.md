---
layout: page
title: Configuration file
---

PipelineBot manages pipelines based on configuration files. One configuration file
describe one pipeline.

# About YAML syntax for pipelines
 
Pipeline files uses YAML syntax, and must have either a `.yml` or `.yaml` file extension. If
you're new to YAML and want to learn more, 
see "[Learn YAML in five minutes](https://www.codeproject.com/Articles/1214409/Learn-YAML-in-five-minutes)"

You must store pipeline files in the `.github/pipelines` directory of your repository.


## Stage
`stages` are top level resource for this file which house a list of stages in the pipeline. 
Documentation for stages is provided below.

```yaml
# .github/pipelines/pipeline0.yml
stages:
  - Build
  - Test
  - Stage
  - Production
```

## Task

`tasks` are another root level resource for the configuration file. It contains a list of
tasks which are bound to a `stage`. Documentation for `tasks` is provided bellow. 

```yaml
# same file: .github/pipeline/pipeline0.yml
tasks:
  - workflow: build
    stage: Build
  - name: test1
    workflow: unit test
    stage: Test
    ...
  - name: deploy to prod
    stage: Production
    ignore: [test1]
```

### name
In Pipeline Bot, `task` is a container of Github's `workflow`. You can use the `name` field
to give the `workflow` an alias. If you omit `name` in your pipeline file, we default it to
`workflow`'s name.

### workflow
**Required** The `workflow` the is contained in the `task` item. The value of the `workflow` field is the 
[workflow name](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#name) in GitHub.

> You can set `name` of your workflow with the name field in your `workflow` files.
 If you omit name in your workflow file, GitHub sets it
 to the workflow file path relative to the root of the repository. In that case, Pipeline Bot
 uses the workflow filename as the workflow name.

### stage
**Required** The `stage` that this `task` belongs to. 
<span class="warning">If not provided, this `task` is ignored.</span>

### ignore
The tasks from the previous stage that should be ignored when starting this task. 
By default, a stage starts only all the tasks of the
previous stage finish successfully. 

If a task has `tasks` in the `ignore` list, it will be started regardless
whether the tasks in the ignore list succeeded or not. However, it will still wait for all the
tasks to finish. 

> #### Road-map (RFC)
> You can also put the previous `stage` in the `ignore` list, the `task` will be started as soon as
the previous `stage` is started.
