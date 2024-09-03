---
title : "Review Reusable Workflows"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 9.4 </b> "
---

As mentioned in the High-Level Design section, there are several reusable jobs in the Release and Rollback workflows.

![0001](/images/9/4/0001.svg?featherlight=false&width=100pc)

![0002](/images/9/4/0002.svg?featherlight=false&width=100pc)

- **Validate version format** job can be reused in both workflows to check [semantic versioning](https://semver.org/) of the project.
- A **Release** job (in the Release workflow) and a **Rollback** job (in the Rollback process) can be combined to create a reusable job.


You now explore the reusable workflows.

#### "Validate format of semantic version" Workflow

Check out **.github/workflows/wc-validate-version-format.yml** file.

```
#  This workflow will be called in release and rollback workflows
name: Validate format of semantic version

on:
  workflow_call:
    inputs:
      version:
        description: "Specify semantic version tag of the Docker image to check."
        required: true
        type: string

jobs:
  validate-version-format:
    name: Validate semantic version format
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          sparse-checkout: |
            .github
          sparse-checkout-cone-mode: false

      - name: Create path to scripts
        id: scripts
        run: |
          echo "path=./.github/scripts" >> $GITHUB_OUTPUT

      - name: Validate format of semantic version ${{ inputs.version }}
        run: |
          chmod +x -R ./.github/scripts
          ${{ steps.scripts.outputs.path }}/validate-version-format.sh ${{ inputs.version }}
```

#### "Deploy" Workflow