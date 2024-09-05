---
title : "Review Update Dependency Cache Workflow"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 9.3 </b> "
---

You now explore the Update dependency cache workflow.

![0001](/images/9/3/0001.svg?featherlight=false&width=100pc)

Check out **.github/workflows/update-cache.yml** file.

```yml
name: Update dependency cache

on:
  push:
    branches: [main]

  workflow_dispatch:

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  update-dependency-cache:
    name: Update dependency cache
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
        id: cache
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper
          key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            ${{ runner.os }}-gradle-
          lookup-only: true

      - if: ${{ steps.cache.outputs.cache-hit != 'true' }}
        name: Update cache
        run: |
          chmod +x gradlew
          ./gradlew dependencies
```

This GitHub Actions workflow is designed to update the dependency cache for a project, specifically for Java Gradle-based projects. Let's take a high-level look at key components of the workflow.

#### Events
**push**: triggers the workflow when the merge group for a given pull request is successful and the PR is actually merged into the *main* branch.

**workflow_dispatch:** allows the workflow to be manually started for debugging without inputs.

#### Concurrency

Ensures that only one instance of the workflow runs for a specific branch at a time, canceling any in-progress runs if a new one starts.

#### Job

**update-dependency-cache**
- Purpose: updates dependency cache in the *main* branch if needed.
- Steps
  - Checkout the code.
  - Set up Java (JDK).
  - If a cache entry already exists, the process of restoring dependency is skipped.
  - This step executes only if the cache is not hit, after which the cache will be refreshed.