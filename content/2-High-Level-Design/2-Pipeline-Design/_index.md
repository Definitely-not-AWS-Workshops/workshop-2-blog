---
title : "Pipeline Design"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

<!-- In this section, you first understand what CI/CD refers to in the workshop. You then examine suitable branching and deployment strategies that help deliver the application reliably and faster. After that, you define the team workflow and CI/CD pipelines using the Business Process Model and Notation 2.0. Lastly, you provide the overall AWS architecture that has all the services needed for the development and delivery of AWSome Books, which are going to be implemented in the hands-on sections.  -->

Dive into this section to explore the way to measure deployment efficiency and discover several components that impact your CI/CD process.

But before we dive in, let's explore what CI/CD is all about in this workshop!

#### Yet another CI/CD Definition!

The CI/CD space contains a lot of contradictory definitions. I might take the definitions from the book [continuous delivery](https://www.amazon.co.uk/Grokking-Continuous-Delivery-Christie-Wilson/dp/1617298255):

-  **Continuous Integration (CI)** is the process of combining code changes frequently, with each change verified on check-in.
-  **Continuous Delivery (CD)** involves being able to release code changes safely at any time, with the process being as simple as pushing a button.

{{% notice note %}}
**Continuous Deployment** is another CD that automatically triggers a deployment to production environment whenever commits are integrated into the *main* branch. While powerful, it requires a mature development process and team to implement effectively. In this workshop, "CD" refers to **Continuous Delivery**.
{{% /notice %}}

To reliably deploy code changes at any time (CD), your codebase must *at least* undergo thorough testing and code reviews through the CI process before being merged into the *main* branch. This makes CI and CD closely connected — you cannot safely deploy until your code is fully verified and ready to perform as expected.

#### The DevOps Research and Assessment (DORA) Metrics

You have probably heard that *"DevOps accelerates the software development process"* quite often, and that CI involves *"frequently integrating code changes"* (as mentioned earlier). But just how fast is fast? Are you delivering software in weeks or months, or could you be delivering in hours?

Organizations embracing DevOps often use various metrics to gauge the success of their CI/CD pipelines, infrastructure automation, testing strategies, and overall development practices. The most popular ones are the [DORA metrics](https://dora.dev/), which focus on evaluating software team performance through 4 key metrics (from [the 2021 DORA report](https://dora.dev/research/2021/dora-report/)). These metrics are divided into 2 main categories: *throughput* and *stability*, providing a comprehensive view of both the speed and reliability of software delivery.

**Throughput**

Throughput measures the velocity of changes that are being made. DORA assesses throughput using the following metrics:

- **Change lead time** - this metric measures the time it takes for a code commit or change to be successfully deployed to production. It reflects the efficiency of your delivery pipeline.
- **Deployment frequency** - this metric measures how often application changes are deployed to production. Higher deployment frequency indicates a more agile and responsive delivery process.

**Stability**

Stability measures the quality of the changes delivered and the team’s ability to repair failures. DORA assesses throughput using the following metrics:

- **Change fail percentage** - this metric measures the percentage of deployments that cause failures in production, requiring hotfixes or rollbacks. A lower change failure rate indicates a more reliable delivery process.
- **Failed deployment recovery time** - this metric measures the time it takes to recover from a failed deployment. A lower recovery time indicates a more resilient and responsive system.

As part of their evaluation, the DORA team also categorized software development teams into four performance tiers: *low*, *medium*, *high*, and *elite*. This ranking system offers a clear perspective on each team’s overall effectiveness and success .

|  Metric  |  Elite  | High  |  Medium  | Low  |
|---|---|---|---|---|
| **Deployment frequency** | multiple deploys per day | Between once per week and once per month  | Between once per month and once every 6 months  | Fewer than once per six months  |
| **Lead time for changes** | Less than one hour | Between one day and one week | Between one month and six months | More than six months |
| **Time to restore service** | Less than one hour | Less than one day | Between one day and one week | More than six months |
| **Change failure rate** | 0%-15% | 16%-30% | 16%-30%| 16%-30% |

Even with the latest and greatest tools, you cannot truly accelerate your software development process without the right team culture — one where every team member understands and follows the practices needed to increase the release rate.

#### Branching Strategy

A branching strategy is a crucial component that affects pipeline design. At the very beginning of a pipeline design, it must be apparent how the team operates and which process they use. Depending on the plan, the pipeline flow varies.

The most popular branching strategies for effective code collaboration include [feature branching](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow), [gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), and [trunk-based development](https://trunkbaseddevelopment.com/). 

As **feature branching** and **gitflow** approaches introduce long-lived branches that stay for days or even longer, they prevent the fast feedback idea of DevOps practices. Merging back these long-lived branches with a huge amount of code changes to the main codebase can result in 

- a slowdown in the code review process due to the large volume of changes being introduced all at once. This can overwhelm reviewers and make it difficult to thoroughly assess the quality of each change.
- a higher chances of code conflicts as multiple developers work on separate features for an extended period. These conflicts can be complex and time-consuming to resolve, further delaying the integration of changes into the main codebase.
- a higher rate of CI failures as the size and complexity of the changes introduce new bugs and errors that may not be immediately apparent. This can disrupt the development workflow and require additional time and effort to diagnose and address issues before they can be merged into the main codebase.

The AWSDSC-XUT team has past experience with *gitflow* branching strategy and its limits from other projects. They, of course, want to apply an alternative branching strategy that overcomes these constraints.  

**Trunk-based development** is considered a companion for DevOps principles. One of the most interesting ideas of this branching strategy is that it requires regular commits to the main branch, even if a feature has not yet been fully developed. The complexity of this branching strategy is relatively low compared to others. *Trunked-based development* comes in both its original form and several variations. 

The main branch is the only branch included in the original form of *trunk-based development*. Changes are pushed straight to the trunk, which then generates release candidates. However, pushing code changes from a local copy of the trunk to the remote trunk without a code review process and CI checks could be highly risky. This means that in the original form of *trunk-based development*, the main branch is not always in a production-ready state. Though frequently pushing code changes to the main codebase may speed up the development process, it may also increase the risk of production outages and cause stress for the team if each software release is unsafe. 

A different variation of *trunk-based development* involves creating short-lived feature branches that can be used to verify code changes before merging them back into the trunk. These branches can last up to a day (or a few days). This sort of branching strategy involves proposing commits to the main branch via pull requests (PRs), conducting CI and code review against those PRs, and then merging the branch and its changes into the main branch once CI passes and PR is approved. Then, the application can be released straight from the main branch or via another release branch created from the main.

The following figure illustrates the idea of *trunk-based development* with short-lived feature branches:

![0001](/images/2/2/0001.svg?featherlight=false&width=32pc)

As previously stated, the general goal of *trunk-based development* is to commit back to the main codebase as frequently as feasible, even if a feature has not yet been fully completed, which may align with the fast feedback of the DevOps culture. You break down your task into small chunks, each of which corresponds to a feature branch and can be completed in less than one day. That being said, it is hard! And it may feel easier not to do it. Despite minimizing difficulties when working with long-lived feature branches on other branching strategies, *trunk-based development* requires experienced teams to control incompleted parts of the codebase. 

Here are some helpful techniques for making small and frequent PRs:

- divide the work into separate tasks that should take a maximum of one day or a few hours. Consider writing brief, stand-alone PRs for them, each with associated documentation and tests.
- work-in-progress features can be hidden from user access by using *feature flags*, or they can be prevented from being compiled into the codebase altogether by using *build flags*.
- if working from a single large feature branch is unavoidable, look for sections that can be committed back and take the time to split and combine the corresponding pull requests.

The benefits of *trunk-based development* with short-lived feature branches allow the AWSDSC-XUT team to deliver AWSome Books more quickly. The team is now eager to start applying the branching strategy, which achieves CI/CD and lowers stress while working with long-lived branches, despite the fact that it requires highly trained engineers to handle its difficulties. 

#### Deployment Strategy

Even with the best tools and a code coverage of 100%, a bug-free trunk is not guaranteed by a CI pipeline; rather, it simply raises your confidence that the main codebase is in a releaseable state. Without a suitable deployment strategy, even with a CI pipeline acting as a quality gate before merging back the PRs to the trunk, it could still be unsafe to deliver your software continually. If you want to release software more often without losing reliability, you may require a deployment strategy that reduces the impact of a production outage.

Production failures occur every two weeks due to the AWSDSC-XUT team's failure to adopt deployment methodologies in previous projects. The projects are non-profit, and the majority of their users are students, infrequent production interruptions are acceptable. To reduce risk and improve trust in users, the AWSome Books team plans to implement a suitable deployment strategy.  As a result, their college peers may be more enthusiastic about using AWSome Books.

The majority of deployment strategies have the common downsides of a lengthy rollback and launching the entire deployment before realizing anything is wrong. There are a number of deployment strategies that reduce stress when software is released. [canary release](https://martinfowler.com/bliki/CanaryRelease.html) (or *canary deployment*) and [blue/green deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html?ref=dombat.co.uk) (or *red-black deployment*) are popular ones. 

In **canary release**, one instance (called the *canary*) is updated with the new version of software, and a small percentage of traffic is directed to it.

![0002](/images/2/2/0002.svg?featherlight=false&width=42pc)

 If the *canary* is in good condition, and there are two possible ways that the deployment can go forward: either by moving all traffic to instances that are running the updated version or by progressively generating more *canary* instances and rerouting all traffic to them until the old instances are no longer receiving any traffic at all. The selling point of the strategy is that only a small percentage of selected users may experience the outage if the *canary* is unhealthy. In that case, the entire process can be stopped and all traffic is routed back to the original instances. It drastically lowers the *time to restore service* in [DORA metrics](https://www.atlassian.com/devops/frameworks/dora-metrics) since the traffic is routed to the original instances all at once. 

In my opinion, although *canary release* is supported by AWS CodeDeploy, it fails to demonstrate the real meaning and potential of the deployment strategy. *canary release* by AWS CodeDeploy allows all software users to have access to the application versions proportionally, rather than just a subset of software users. The real power of *canary release* comes from the ability to select which users will receive the new version.

**Blue/Green Deployment** is another deployment strategy supported by AWS CodeDeploy. With *blue/green deployment*, you build a completely new set of instances and update them. Only after those instances are ready do you move traffic from the old (the "blue" instances, but it is not really important which ones are which color) to the new (the "green" instances).

Both deployment strategies significantly reduce application downtime during deployments and rollbacks by rerouting traffic between old and new versions. However, the main difference lies in the potential impact during an outage. With a *blue/green deployment*, all users might experience downtime if an issue arises. In contrast, a *canary release* restricts the impact to only a small subset of users.

For deploying AWSome Books, you are going to use the *blue/green deployment* strategy, as implementing a *canary release* can require substantial effort in a real-world scenario. AWS CodeDeploy with *blue/green deployment* streamlines the process by offering a ready-made, production-grade solution, saving you the time and complexity of building one from scratch.

 

#### Business Process Model and Notation (BPMN) 2.0

[BPMN](https://www.omg.org/spec/BPMN#document-metadata) - has become the de-facto standard for business processes diagrams (you should be familiar with it if you are a Business Analyst). It is meant for usage directly by stakeholders who create, manage, and implement business processes. A pipeline design defines the orchestration of a process or workflow and has many analogies to modeling business processes. The business process modeling paradigm can thus serve as the basis for pipeline design. BPMN diagrams used to build workflows or CI/CD pipelines provide insight into software delivery processes, such as stages, activities, and relationships with other systems. 

The notation used in this workshop is BPMN 2.0. BPMN 2.0 has a unique notation with identifiable icons known as *elements*. The table below shows a subset of the most used BPMN 2.0 elements that might be enough for designing your pipelines.

![0003](/images/2/2/0003.svg?featherlight=false&width=40pc)

#### The Proposal Workflow and Pipelines

Since AWS CodePipeline does not support stage execution in parallel


In the next section, you learn how to improve the pipelines

#### AWS Architecture