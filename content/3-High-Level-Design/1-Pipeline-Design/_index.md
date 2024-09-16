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

- *seamless integration:* directly integrated with GitHub repositories, enabling smooth automation of development processes.
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

#### The Proposal Workflow and Pipelines

Let's have a look at how the AWSome Books development and integration processes would work.

**Local Development**

![0004](/images/3/1/0004.svg?featherlight=false&width=100pc)


To align with DevOps' fast feedback principles, which emphasize the need to detect issues as soon as possible, your initial code change verifications should begin with *feature* branches on your local machine. [Test-Driven Development](https://martinfowler.com/bliki/TestDrivenDevelopment.html) (TDD), where tests are developed before the actual code and your code changes should be performed locally first to discover issues early before publishing a pull request, is one of the software development methodologies that couple particularly well with DevOps practices.

During the hands-on sections, you just skip this phase. It is essential to incorporate TDD into your development process in real-world circumstances. 

**Integration of Code Changes to the Remote Repository**

![0005](/images/3/1/0005.svg?featherlight=false&width=100pc)

**1.** After verifying that your code changes pass all tests locally on the short-lived *feature* branch, you are ready to commit and push them to the corresponding remote branch.

**2.** On the remote short-lived *feature* branch, you might create a pull request (PR).

**3.** The PR creation might trigger a GitHub Actions CI workflow execution. While the *Scan image* job depends on the *Build image* job, you may have noticed that the *Run unit tests*, *Run integration tests*, *Scan source code*, and *Build image* are independent and execute in parallel to reduce the CI workflow execution time.

**4a.** In the event of any job failures, your CI workflow should send a notification to the Slack channel and conclude with a failed status.

**4b.** If all jobs complete successfully, your CI workflow sends a notification to the Slack channel before wrapping up. You then move to the next step.

**5a.** Make sure your team members review and approve the PR. If they have not done so already, they should conduct a code review before moving on to step **6**.

**5b.** If your PR has been reviewed and approved, merge the PR to the *main* (*trunk*) branch. Proceed to step **7**.

**6a.** If the PR is not approved, start by reviewing the issues and comments. Resolve these on your local branch, then recommit and push the updated code changes as step **1**.

**6b.** Otherwise, you can add the PR to the merge queue. This will trigger another CI workflow to validate the code changes from the *feature* branch integrated with the *main* branch. You might need to revisit step **3**.

**7.** Merging the PR might trigger an Update dependency cache workflow, which writes the dependency cache to the GitHub Actions Cache storage for the *main* branch.

![0003](/images/3/1/00070.svg?featherlight=false&width=100pc)

![0003](/images/3/1/00071.svg?featherlight=false&width=100pc)

In the next section, you learn how to improve the pipelines

