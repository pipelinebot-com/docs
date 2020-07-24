---
layout: page
title: Guide
group: Guide
---

# Guide

The official Pipeline guide over the next sections will go through setting
up a production grade deployment pipeline for an organization using PipelineBot
and other GitHub tools.

{%- for page in site.pages %}
{%- if page.group == "Guide" and page.title != "Guide" %}
1. [{{ page.title }}]({{page.url}})
{%- endif %}
{%- endfor %}

## What we'll be doing

We'll be setting up a pipeline that deploys code using the following workflow:

```                                                                                       
+----------+           +----------+            +------------+              +-------------+  
|    CI    |---Auto--->|   UAT    |----Auto--->|  Staging   | ---Manual--->| Production  |  
+----------+           +----------+            +------------+              +-------------+ 
```

At the end of this we'll have a repository that looks like this example
<https://github.com/pipelinebotapp/example> if you want to jump ahead.

This is a typical and safe workflow for most organizations to follow. When a
 developer spins up a pull request we'll do the typical CI workflows with
 lint, unit test and build. After that, the package is deployed to a test environment
 to run integration test and user acceptance test (UAT). Once it passes, the same package is 
 deployed to staging environment as a release candidate. At anytime the team can choose
 a version from the release candidates and manually deploy to production.

There are all kinds of variance, but concept tends to be similar.

## How this guide works

This guide is agnostic to how and where you run the workflows and instead focuses on the
processes around your deployment practices. PipelineBot decouples the
delivery processes from the underlying technology.

All of the workflows will use stubbed GitHub actions since this is the
simplest way to get up and running. However, since everything is just responding
to a GitHub webhook, your self-hosted runner is fully capable of executing all of these
workflows.

We will also link in the footer of the guide to the available actions
 that will allow you to wire up actual deployments and tests
 throughout the process.

Let's get started :)

## First steps

1. Create a blank GitHub repository that we can use for the guide.
2. [Install the PipelineBot GitHub App][app]{:target="_blank"} on the repo.

## Next

[Start with PR configuration >>](/docs/guide/1-pr-ci)

[app]: {{site.start_url}}
