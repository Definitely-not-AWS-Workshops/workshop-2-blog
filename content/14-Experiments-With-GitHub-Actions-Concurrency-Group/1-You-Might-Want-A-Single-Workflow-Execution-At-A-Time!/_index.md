---
title : "You Might Want A Single Workflow Execution At A Time!"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 14.1 </b> "
---

#### Problems

By default, GitHub Actions allows multiple jobs within the same workflow, multiple workflow runs within the same repository, and multiple workflow runs across a repository owner's account to run concurrently. This means that multiple workflow runs, jobs, or steps can run at the same time.

Have you ever encountered any of these challenges during your software development process!

**Running Tests on Feature Branches**

- *Scenario:* your team is working on multiple feature branches, and each time code is pushed, a test suite is triggered on a CI environment.

- *Problem:* when a developer pushes multiple commits in quick succession, tests will run for every push. However, the team is only interested in the test results for the latest commit.

**Continuous Deployment to Production Environment**

- *Scenario:* you have a CI/CD pipeline that automatically deploys to a production environment after passing tests.

- *Problem:* multiple developers push updates to the repository in a short span, triggering multiple deployments to production. This could cause a race condition where one deployment overwrites another or causes downtime.

<!-- *Solution:* use a concurrency group to ensure only one deployment is running at a time for a given environment. If a new commit is pushed, the ongoing deployment can be canceled, and only the latest version is deployed. -->

Allowing multiple pipeline executions simultaneously can lead to several potential drawbacks:

- *Duplicate Workflows:* multiple jobs run simultaneously for the same task (e.g., multiple deployments, tests, or builds), leading to redundant or wasted effort.
  
- *Resource Waste:* unnecessary consumption of CI/CD resources (e.g., CPU, memory, storage) due to running duplicate jobs or workflows.
  
- *Conflicts:* jobs modifying the same resources or environments simultaneously, causing race conditions, inconsistent states, or overwriting each other's work.

#### Control the Concurrency of Workflows and Jobs

GitHub Actions also provides the flexibility to control concurrency with [concurrency group](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/control-the-concurrency-of-workflows-and-jobs), ensuring that only one workflow run, job, or step is executed at a time in specific contexts. This feature can be crucial for managing resources efficiently, avoiding conflicts, and preventing unexpected overuse of Actions minutes and storage when running multiple workflows simultaneously.

Therefore, with *concurrency group*,

- **Continuous Deployment to Production Environment** can be streamlined by utilizing a *concurrency group*, which ensures that only one deployment is active at any given time for a specific environment. If a new commit is pushed, the current deployment can be canceled, allowing only the latest version to be deployed seamlessly.

- **Running Tests on Feature Branches** can be enhanced with a *concurrency group*, which automatically cancels any ongoing tests when a new commit is pushed to the same branch. This ensures that only the most recent changes are evaluated.