---
title : "Review CI Workflow"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 11.3 </b> "
---

You now explore the CI workflow.

![0001](/images/11/3/0001.svg?featherlight=false&width=100pc)

Check out **.github/workflows/ci.yml** file.

```yml
name: CI

on:
  pull_request:
    branches: [main]
    types: [opened, synchronize, reopened]
    paths-ignore:
      - docs/**
      - docker-compose.dev.yml

  merge_group:
    branches: [main]

  workflow_dispatch:

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

permissions:
  id-token: write
  contents: read

env:
  CI_IMAGE_NAME: localbuild/ci-image/latest

jobs:
  unit-test:
    name: Run unit tests
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up JDK ${{ vars.JAVA_VERSION }}
        uses: actions/setup-java@v4
        with:
          java-version: ${{ vars.JAVA_VERSION }}
          distribution: ${{ vars.JAVA_DISTRIBUTION }}

      - uses: actions/cache@v4
        name: Cache dependencies
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper
          key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            ${{ runner.os }}-gradle-

      - name: Run unit tests
        run: |
          chmod +x gradlew
          ./gradlew test

      - if: ${{ !cancelled() }}
        name: Upload unit test report
        uses: actions/upload-artifact@v4
        with:
          name: unit-test-report
          path: build

  integration-test:
    name: Run integration tests
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up JDK ${{ vars.JAVA_VERSION }}
        uses: actions/setup-java@v4
        with:
          java-version: ${{ vars.JAVA_VERSION }}
          distribution: ${{ vars.JAVA_DISTRIBUTION }}

      - uses: actions/cache@v4
        name: Cache dependencies
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper
          key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            ${{ runner.os }}-gradle-

      - name: Run integration tests
        run: |
          chmod +x gradlew
          ./gradlew integrationTest

      - if: ${{ !cancelled() }}
        name: Upload integration test report
        uses: actions/upload-artifact@v4
        with:
          name: integration-test-report
          path: build

  scan-source-code:
    name: Scan source code
    runs-on: ubuntu-latest
    permissions:
      contents: read
      security-events: write
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up JDK ${{ vars.JAVA_VERSION }}
        uses: actions/setup-java@v4
        with:
          java-version: ${{ vars.JAVA_VERSION }}
          distribution: ${{ vars.JAVA_DISTRIBUTION }}

      - uses: actions/cache@v4
        name: Cache dependencies
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper
          key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            ${{ runner.os }}-gradle-

      - name: Run build
        run: |
          chmod +x gradlew
          ./gradlew build -x test

      - name: Code vulnerability scanning
        uses: anchore/scan-action@v3
        id: scan
        with:
          path: ${{ github.workspace }}
          fail-build: false
          severity-cutoff: high

      - if: ${{ !cancelled() }}
        name: Upload vulnerability report
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: ${{ steps.scan.outputs.sarif }}
          category: source-code-scanning-report

  build-image:
    name: Build image
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
          tags: ${{ env.CI_IMAGE_NAME }}
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
          image: ${{ env.CI_IMAGE_NAME }}
          fail-build: false
          severity-cutoff: high

      - if: ${{ !cancelled() }}
        name: Upload vulnerability report
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: ${{ steps.scan.outputs.sarif }}
          category: image-scanning-report
```

This GitHub Actions workflow is designed to perform CI jobs when specific events occur on the repository. Code review can only begin once all jobs in the CI workflow have successfully passed. Let's take a high-level look at key components of the workflow.

#### Events

**pull_request:** triggers the workflow for pull requests targeting the *main* branch, specifically when they are *opened*, *synchronized*, or *reopened*. It ignores changes to documentation and certain configuration files.

**merge_group:** triggers the workflow when merge groups are created on the main branch.

**workflow_dispatch:** allows the workflow to be manually started for debugging without inputs.
  
#### Concurrency

Ensures that only one instance of the workflow runs for a specific pull request at a time, canceling any in-progress runs if a new one starts for that pull request.
  
#### Jobs

In general,

**unit-test**, **integration-test**, **scan-source-code**, and **build-image** jobs can run in parallel to reduce overall workflow execution time because they are independent of each other.

**unit-test**, **integration-test**, and **scan-source-code** jobs might restore dependency cached from the *main* branch to speed up execution time of each job.

Have a look at what each job does,

**unit-test**
- Purpose: runs unit tests on the codebase.
- Steps
  - Checkout the code.
  - Set up Java (JDK).
  - Restore dependency cache.
  - Run unit tests.
  - Upload the unit test report for review later if the tests are not canceled. 

**integration-test**
- Purpose: runs integration tests.
- Steps
  - Checkout the code.
  - Set up Java (JDK).
  - Restore dependency cache.
  - Run integration tests.
  - Upload the integration test report for review later if the tests are not canceled. 

**scan-source-code**
- Purpose: scans the source code for vulnerabilities.
- Steps:
  - Checkout the code.
  - Set up Java (JDK).
  - Restore dependency cache.
  - Build the project without running tests.
  - Perform code vulnerability scanning.
  - Upload the vulnerability report for review later if the scanning are not canceled.

**build-image**
- Purpose: builds a Docker image for the application.
- Steps:
  - Checkout the code.
  - Set up Docker Buildx.
  - Build the Docker image and prepare it for vulnerability scanning.
  - Upload the built image as an artifact.

**scan-image**
- Purpose: scans the Docker image for vulnerabilities.
- Depends on: **build-image** job. This job might wait for the build-image job to be successful before running.
- Steps:
  - Download the image artifact built in job **build-image**.
  - Load the Docker image to Docker engine.
  - Perform image vulnerability scanning.
  - Upload the vulnerability report for review later if the scanning are not canceled.