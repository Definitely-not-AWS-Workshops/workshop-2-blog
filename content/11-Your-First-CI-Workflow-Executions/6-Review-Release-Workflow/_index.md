---
title : "Review Release Workflow"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 11.6 </b> "
---

You now explore the Release workflow.

![0001](/images/11/6/0001.svg?featherlight=false&width=100pc)

Check out **.github/workflows/release.yml** file.

```yml
name: Release

on:
  push:
    tags:
      - "v*.*.*"

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}

env:
  TEST_IMAGE_NAME: localbuild/prepared-image:latest

permissions:
  id-token: write
  contents: read

jobs:
  validate-version-format:
    name: Validate semantic version format
    uses: ./.github/workflows/wc-validate-version-format.yml
    with:
      version: ${{ github.ref_name }}

  build-image:
    name: Build image
    needs: [validate-version-format]
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Build image for vulnerability scanning
        uses: docker/build-push-action@v5
        with:
          context: .
          target: production
          load: true
          tags: ${{ env.TEST_IMAGE_NAME }}
          outputs: type=docker,dest=/tmp/prepared-image.tar

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: prepared-image
          path: /tmp/prepared-image.tar

  scan-image:
    name: Scan image
    needs: [build-image]
    runs-on: ubuntu-latest
    permissions:
      contents: read
      security-events: write
    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: prepared-image
          path: /tmp

      - name: Load image
        run: docker load --input /tmp/prepared-image.tar

      - name: OCI image vulnerability scanning
        uses: anchore/scan-action@v3
        id: scan
        with:
          image: ${{ env.TEST_IMAGE_NAME }}
          fail-build: false
          severity-cutoff: high

      - if: ${{ !cancelled() }}
        name: Upload vulnerability report
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: ${{ steps.scan.outputs.sarif }}
          category: image-scanning-report

  release:
    name: Release
    needs: [scan-image]
    uses: ./.github/workflows/wc-deploy.yml
    with:
      aws-region: ${{ vars.AWS_REGION }}
      role-to-assume: ${{ vars.ROLE_TO_ASSUME }}
      download-artifact-name: prepared-image
      download-artifact-path: /tmp
      ecr-repository: ${{ vars.PROJECT }}
      image-tag: ${{ github.ref_name }}
      task-definition: ${{ vars.PROJECT }}
      container-name: ${{ vars.PROJECT }}
      ecs-cluster: ${{ vars.ECS_CLUSTER }}
      ecs-service: ${{ vars.PROJECT }}
      codedeploy-application: ${{ vars.CODEDEPLOY_APPLICATION }}
      codedeploy-application-group: ${{ vars.CODEDEPLOY_APPLICATION_GROUP }}
```

This GitHub Actions workflow is designed to perform release tasks when specific events occur on the repository. Let's take a high-level look at key components of the workflow.

#### Events
- **push**: triggered when a push event happens on any tag matching the pattern v*.*.* (typically indicating semantic versioning tags like v1.0.0)

#### Concurrency

Ensures that only one instance of this workflow runs for a given tag at a time, identified by the workflow name and reference. You might not want to run multiple releases in at the same time.

#### Jobs
**validate-version-format**:
- This job reuses jobs or steps defined in the workflow **.github/workflows/wc-validate-version-format.yml**
- Validates the format of the version tag to ensure it follows semantic versioning.

**build-image**
- Builds a Docker image for the application.
- Steps:
  - Checkout the code.
  - Set up Docker Buildx.
  - Build the Docker image and prepare it for vulnerability scanning.
  - Upload the built image as an artifact.

**scan-image**
- Scans the Docker image for vulnerabilities.
- Depends on: **build-image** job. This job might wait for the build-image job to be successful before running.
- Steps:
  - Download the image artifact built in job **build-image**.
  - Load the Docker image to Docker engine.
  - Perform image vulnerability scanning.
  - Upload the vulnerability report for review later if the scanning are not canceled.