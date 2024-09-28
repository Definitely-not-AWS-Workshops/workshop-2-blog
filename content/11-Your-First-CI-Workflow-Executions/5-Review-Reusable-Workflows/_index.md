---
title : "Review Reusable Workflows"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 11.5 </b> "
---

Both the Release and Rollback workflows leverage reusable jobs for efficiency:

- The *Validate version format* job is ideal for reuse in both workflows to ensure consistency when handling version inputs.
- The *Release* and *Rollback* jobs, which interact with AWS ECS and AWS CodeDeploy for deploying and rolling back project versions, can be streamlined into a reusable component through the Deploy workflow.
   

![0001](/images/11/5/0001.svg?featherlight=false&width=100pc)

![0002](/images/11/5/0002.svg?featherlight=false&width=100pc)

<!-- You now explore the reusable workflows.

#### "Validate format of semantic version" Workflow

Check out *.github/workflows/wc-validate-version-format.yml* file.

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

#### "Deploy" Workflow -->