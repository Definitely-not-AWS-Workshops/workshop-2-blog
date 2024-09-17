---
title : "Pipeline Design"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---
 
Let's first take a look at several tools that the AWSDSC-XUT team may find useful for the deployment and collaboration of AWSome Books. The CI, Release, and Rollback processes for the project are then examined.

#### GitHub Actions

![0001](/images/3/1/0001.png?featherlight=false&width=4pc)

[GitHub Actions](https://github.com/features/actions) is a robust CI/CD and automation tool embedded within GitHub, designed to streamline the development workflow. Here are its key features:

- *seamless integration with GitHub:* directly integrated with GitHub repositories, enabling smooth automation of development processes.
- *custom workflows:* create and manage workflows using YAML configuration files to automate tasks such as building, testing, and deploying code.
- *extensive marketplace:* access a wide range of pre-built actions for various tasks, enhancing functionality and saving time.
- *cross-platform support:* run workflows across multiple operating systems and environments either GitHub-hosted or self-hosted, including Linux, Windows, and macOS.
- *complex workflow management:* utilize features like conditions and matrix builds to handle sophisticated automation scenarios efficiently.

The AWSDSC-XUT team is well-versed in using GitHub but has yet to explore GitHub Actions. Given its seamless integration with GitHub and powerful features, the team is excited to leverage GitHub Actions for building their CI/CD pipelines.

#### Slack

![0002](/images/3/1/0002.png?featherlight=false&width=6pc)

Prior to adopting Slack, the AWSDSC-XUT team faced several challenges in collaboration:

- *siloed communication:* communication between the team members are usually fragmented across different tools. In other words, without Slack and ChatOps, team members often switch between various tools (e.g., GitHub, AWS) to perform tasks like checking system health or deployment status.

- *delayed feedback loops:* it does not provide immediate feedback for tasks such as deployments, incident response, or code merges. Team members cannot see logs, results, and errors right in the chat, increasing delays caused by waiting for email notifications or checking dashboards.

[Slack](https://slack.com/) simplifies [ChatOps](https://www.atlassian.com/incident-management/devops/chatops). The combination addresses the challenge of improving communication and collaboration in software development, IT operations, and DevOps processes. The core problems it solves include:

- *operational visibility:* all stakeholders can view the execution of tasks like deployments, system health checks, or incident handling in real-time, fostering transparency and alignment among teams.

- *incident response efficiency:* during outages or incidents, teams can use ChatOps to trigger remediation actions, gather metrics, or coordinate responses without leaving Slack, improving response times.

- *automation:* teams can automate repetitive tasks, such as triggering builds or deploying applications, directly from Slack, improving efficiency and reducing human error.

While ChatOps offers extensive capabilities, the AWSDSC-XUT team could start by experimenting with a simple approach — integrating deployment and pipeline notifications into Slack. This would provide valuable insights and a streamlined communication process in one place.

Before diving into CI/CD design, it is helpful to familiarize yourself with some fundamental components of Business Process Model and Notation (BPMN) that are used in design diagrams. This will provide you with a clearer understanding of the overall design.

#### Business Process Model and Notation (BPMN) 2.0

[BPMN](https://www.omg.org/spec/BPMN#document-metadata) - has become the de-facto standard for business processes diagrams (you should be familiar with it if you are a Business Analyst). It is meant for usage directly by stakeholders who create, manage, and implement business processes. A pipeline design defines the orchestration of a process or workflow and has many analogies to modeling business processes. The business process modeling paradigm can thus serve as the basis for pipeline design. BPMN diagrams used to build workflows or CI/CD pipelines provide insight into software delivery processes, such as stages, activities, and relationships with other systems. 

The notation used in this workshop is BPMN 2.0. BPMN 2.0 has a unique notation with identifiable icons known as *elements*. The table below shows a subset of the most used BPMN 2.0 elements that might be enough for designing your pipelines.

![0003](/images/3/1/0003.svg?featherlight=false&width=40pc)

#### From Local Development to Integration of Code Changes to the Remote Main Branch

The AWSDSC-XUT team may want to experiment with a simplified *trunk-based development* branching strategy that consists of the *main* (*trunk*) branch and several short-lived *feature* branches. All thoroughly tested code changes should be merged via pull requests and reviewed by your team members before being integrated into the *main* branch. This minimal setup allows for direct deployment of the AWSome Books application from the *main* branch at any time. You can refer to [trunk-based development](https://trunkbaseddevelopment.com/) for other advanced variants of this branching strategy or design your own for a customized branching strategy that nevertheless adheres to DevOps practices.

{{% notice tip %}}
While pure DevOps practices emphasize automation and aim to minimize the need for manual testing or multiple environments beyond production, real-world scenarios often require a more nuanced approach. In practice, teams typically manage several environments (dev, testing, UAT, production, etc.), incorporating manual testing to ensure everything runs smoothly before the final release to customers.
{{% /notice %}}


Let's have a look at how the AWSome Books local development and integration processes would work.

**Local Development**

In general, writing test cases should be the first step in the local development process for AWSome Books. You may then run unit tests. You may want to refactor the well-tested code changes you made before rerunning the tests.

![0004](/images/3/1/0004.svg?featherlight=false&width=100pc)

In other words, to align with DevOps' fast feedback principles, which emphasize the need to detect issues as soon as possible, your initial code change verifications should begin with *feature* branches on your local machine. [Test-Driven Development](https://martinfowler.com/bliki/TestDrivenDevelopment.html) (TDD), where tests are developed before the actual code and your code changes should be performed locally first to discover issues early before creating a pull request, is one of the software development methodologies that couple particularly well with DevOps practices.

{{% notice tip %}}
In addition to unit tests, you may also carry out different static and dynamic code analyses if needed. Remember to keep the analysis execution duration as reasonable as possible in order to get quick feedback. Extended analyses should be saved for later phases.
{{% /notice %}}

During the hands-on sections, you just skip this phase. It is essential to incorporate TDD into your development process in real-world circumstances. 

**Integration of Code Changes to the Remote Main Branch**

![0005](/images/3/1/0005.svg?featherlight=false&width=100pc)

**1.** After verifying that your refactored code changes pass all tests locally on the short-lived *feature* branch, you are ready to commit and push them to the corresponding remote branch.

**2.** On the remote short-lived *feature* branch, you might create a pull request (PR).

**3.** The PR creation might trigger a GitHub Actions CI workflow execution. While the *Scan image* job depends on the *Build image* job, you may have noticed that the *Run unit tests*, *Run integration tests*, *Scan source code*, and *Build image* are independent and execute in parallel to reduce the CI workflow execution time as a whole.

- In the event of any job failures, your CI workflow should send a notification to the Slack channel and conclude with a failed status.

- If all jobs complete successfully, your CI workflow sends a notification to the Slack channel before wrapping up. You then move to the next step.

**4** Make sure your team members review and approve the PR.

- If they have not done so already, they should conduct a code review before moving on to step **5**.

- If your PR has been reviewed and approved, merge the PR to the *main* (*trunk*) branch. Proceed to step **6**.

**5.**

- If the PR is not approved, start by reviewing the issues and comments. Resolve these on your local branch, then recommit and push the updated code changes as step **1**.

- Otherwise, you can add the PR to the [merge queue](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-a-merge-queue) (or merge group), which helps increase velocity by automating PR merges into a busy branch and ensuring the branch is never broken by incompatible changes (see [13. Experiments With GitHub Actions Merge Group](13-experiments-with-gitHub-actions-merge-group) for more experiments on why you might want to use this feature). This will trigger another CI workflow execution to validate the code changes from the *feature* branch integrated with the *main* branch. You might need to revisit step **3**.

**6.** If the cache key has changed or the cache entry does not exist, merging the PR may cause an Update dependency cache workflow to start, which automatically uploads the dependency cache to the GitHub Actions Cache storage for the *main* branch.  (see [11. Your First CI Workflow Executions](11-your-first-ci-workflow-executions) for more experiments showing how caching reduces your pipeline execution time).


{{% notice tip %}}
It is necessary to use techniques to shorten the pipeline running time since modern CI/CD technologies factor execution time into the pricing model. Among the various pipeline optimization techniques, dependency caching is one to take into account in the workshop.
{{% /notice %}}

#### Release Process

Let's examine the Release process, which enables you to make available the most recent version of AWSome Books to AWS.

![0006](/images/3/1/0006.svg?featherlight=false&width=100pc)

**1.** To begin, you create a tag with the version that you plan to release using the [Semantic Versioning](https://semver.org/) (SemVer) format.

**2.** The Release workflow may be triggered by first running the *Validate version format* job, ensuring the version adheres to the SemVer format.

- If the version format is valid, proceed to the next step.
- Otherwise, the workflow will send an alert to the Slack channel and stop the execution.

**3.** The *Build image* job creates a Docker image, which may be necessary for your application to operate properly on AWS.

- If the image is built successfully, proceed to the next step.
- If not, the workflow will notify the Slack channel and terminate the execution.

**4.** You can now perform another image analysis to check for any updates to the [Common Vulnerabilities and Exposures](https://jfrog.com/learn/devsecops/cve/) (CVE) database.

- If it finds no vulnerabilities, move on to the following step.
- If not, the workflow will stop the execution and alert the Slack channel.

**5.** Your *Release* job delivers the AWSome Books using the version that was chosen in step **1**. Part of this job is uploading the container image to the AWS ECR. If you want to, you may think about splitting the image publishing to another job. Whether the process succeeds or fails, it will then stop execution and send a notification to the Slack channel.

You may have noticed that the jobs in the workflow are interdependent and must be completed in the correct order. Any job failure has the potential to bring down the Release workflow as a whole. The workflow will alert the Slack channel and then finish, whether it is successful or not.

#### Rollback Process

Let's look at the Rollback process, which lets you use GitHub Actions to manually roll back to earlier versions.

![0007](/images/3/1/0007.svg?featherlight=false&width=100pc)

**1.** To begin, identify the version you want to manually roll back to in SemVer format (such as step **1** in the Release process).

**2.** The Release workflow may be triggered by first running the *Validate version format* job, ensuring the version adheres to the SemVer format.

- If the version format is valid, proceed to the next step.
- Otherwise, the workflow will send an alert to the Slack channel and stop the execution.

**3.** This job verifies whether the version exists.

- If it does, proceed to the next step.
- If not, the workflow will notify the Slack channel and terminate the execution.

**4.** At this point, your *Rollback* job should revert the specified version from step **1**. Regardless of whether it passes or fails, the workflow then sends the notification to the Slack channel and stop the execution.

You may have noticed that the jobs in the workflow are interdependent and must be completed in the correct order. Any job failure has the potential to bring down the Rollback workflow as a whole. The workflow will alert the Slack channel and then finish, whether it is successful or not.

{{% notice tip %}}
For advanced incident management, prioritize automated rollback techniques, keeping manual rollbacks as a secondary option.
{{% /notice %}}
