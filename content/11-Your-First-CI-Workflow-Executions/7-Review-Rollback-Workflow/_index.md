---
title : "Review Rollback Workflow"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 11.7 </b> "
---

You now explore the Rollback workflow.

![0001](/images/11/7/0001.svg?featherlight=false&width=100pc)

Check out *.github/workflows/rollback.yml* file.

```yml
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

permissions:
  id-token: write
  contents: read

jobs:
  validate-version-format:
    name: Validate semantic version format
    uses: ./.github/workflows/wc-validate-version-format.yml
    with:
      version: ${{ inputs.version }}

  check-version-exists:
    name: Check semantic version exists on AWS ECR repository
    needs: [validate-version-format]
    runs-on: ubuntu-latest
    env:
      ECR_REPOSITORY: ${{ vars.PROJECT }}
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          sparse-checkout: |
            .github
          sparse-checkout-cone-mode: false

      - name: Set permissions to run scripts
        run: chmod +x -R ./.github/scripts

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-region: ${{ vars.AWS_REGION }}
          role-to-assume: ${{ vars.ROLE_TO_ASSUME }}

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Check version ${{ inputs.version }} existed in AWS ECR repository ${{ env.ECR_REPOSITORY }}
        run: ./.github/scripts/check-version-exists.sh ${{ env.ECR_REPOSITORY }} ${{ inputs.version }}

  rollback:
    name: Rollback
    needs: [check-version-exists]
    uses: ./.github/workflows/wc-deploy.yml
    with:
      rollback: true
      aws-region: ${{ vars.AWS_REGION }}
      role-to-assume: ${{ vars.ROLE_TO_ASSUME }}
      ecr-repository: ${{ vars.PROJECT }}
      image-tag: ${{ inputs.version }}
      task-definition: ${{ vars.PROJECT }}
      container-name: ${{ vars.PROJECT }}
      ecs-cluster: ${{ vars.ECS_CLUSTER }}
      ecs-service: ${{ vars.PROJECT }}
      codedeploy-application: ${{ vars.CODEDEPLOY_APPLICATION }}
      codedeploy-application-group: ${{ vars.CODEDEPLOY_APPLICATION_GROUP }}
```

This GitHub Actions workflow automates the rollback of an ECS deployment to a specified version using AWS services. Let's take a high-level look at key components of the workflow.

#### Events
- **workflow_dispatch**: this enables manual triggering of the workflow through the GitHub UI. It accepts an input parameter *version*. The user might provide the semantic version (e.g., v1.0.0) they want to rollback to. This is required for the Rollback workflow.

#### Concurrency

Ensures that only one instance of this workflow runs for a given branch at a time, identified by the workflow name and reference. You might not want to run multiple rollbacks in at the same time (explore more about *concurrency group* in [14. Experiments With GitHub Actions Concurrency Group](14-experiments-with-gitHub-actions-concurrency-group)).

#### Jobs
**validate-version-format**:
- This job reuses jobs or steps defined in the workflow **.github/workflows/wc-validate-version-format.yml**
- Validates the format of the version tag to ensure it follows semantic versioning.

**check-version-exists**
- Check semantic version exists on AWS ECR repository.
- Steps:
  - Checkout the code.
  - Configure AWS Credentials
  - Login to Amazon ECR
  - Use the AWS CLI to verify if the specified version exists.

**rollback**

- This job reuses jobs or steps defined in the workflow **./.github/workflows/wc-deploy.yml**.
- It essentially automates the rollback of an ECS service using AWS resources defined in the workflow.