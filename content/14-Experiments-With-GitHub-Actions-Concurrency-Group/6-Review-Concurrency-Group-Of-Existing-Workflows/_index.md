---
title : "Review Concurrency Group Of Existing Workflows"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 14.6 </b> "
---

The AWSome Books codebase already includes *concurrency group*s set up for the GitHub Actions workflows. Let’s take a closer look!

With the *concurrency group* named **${{ github.workflow }}-${{ github.ref }}**, this specifies a unique group identifier for the workflow runs. It combines the name of the workflow **${{ github.workflow }}** with the Git reference **${{ github.ref }}**, which could be a branch or a tag. This means that all runs of a specific workflow for the same branch or tag will be grouped together, preventing them from running concurrently.

To ensure you are always working with the most recent CI checks or dependency updates, it is often beneficial to automatically cancel any ongoing workflows in the same *concurrency group*. With **cancel-in-progress** enabled for your CI and Update dependency cache workflows, any previously running instance will be stopped as soon as a new one is triggered, allowing you to focus only on the most recent run.

**.github/workflows/ci.yml**

```yml {linenos=table,hl_lines=["17-20"],linenostart=1}
name: CI

on:
  pull_request:
    branches: [main]
    types: [opened, synchronize, reopened]
    paths-ignore:
      - docs/**
      - README.md
      - docker-compose.dev.yml

  merge_group:
    branches: [main]

  workflow_dispatch:

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

**.github/workflows/update-cache.yml**

```yml {linenos=table,hl_lines=["9-11"],linenostart=1}
name: Update dependency cache

on:
  push:
    branches: [main]

  workflow_dispatch:

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

For Release and Rollback workflows, canceling a running workflow can be risky and potentially disruptive during deployment. Instead, it is often better to leave **cancel-in-progress** disabled, allowing the most recent workflow to remain pending while another is still running. This ensures that critical processes are not interrupted, providing a smoother and safer deployment experience.

**.github/workflows/release.yml**

```yml {linenos=table,hl_lines=["8-9"],linenostart=1}
name: Release

on:
  push:
    tags:
      - "v*.*.*"

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
```

**.github/workflows/rollback.yml**

```yml {linenos=table,hl_lines=["12-13"],linenostart=1}
name: Rollback

on:
  # Allow manual rollback
  workflow_dispatch:
    inputs:
      version:
        description: Specify the semantic version to rollback to, in the format "v*.*.*" (e.g., "v0.0.1")
        type: string
        required: true

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
```

