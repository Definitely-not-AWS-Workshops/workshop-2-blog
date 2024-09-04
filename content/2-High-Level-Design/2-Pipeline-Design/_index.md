---
title : "Pipeline Design"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

In this section, you first understand what CI/CD refers to in the workshop. You then examine suitable branching and deployment strategies that help deliver the application reliably and faster. After that, you define the team workflow and CI/CD pipelines using the Business Process Model and Notation 2.0. Lastly, you provide the overall AWS architecture that has all the services needed for the development and delivery of AWSome Books, which are going to be implemented in the hands-on sections. 

#### Yet another CI/CD Definition!

The CI/CD space contains a lot of contradictory definitions. I might take the definitions from the book [continuous delivery](https://www.amazon.co.uk/Grokking-Continuous-Delivery-Christie-Wilson/dp/1617298255):

-  **Continuous integration (CI)** is the process of combining code changes frequently, with each change verified on check-in.
-  **Continuous Delivery (CD)** involves being able to release changes to your software safely at any time, with the process being as simple as pushing a button.

{{% notice note %}}
**Continuous Deployment** is another CD that involves automatically delivering on every commit. In other words, when your commit is merged to the *main* branch, it will be instantly deployed to the production environment without human intervention. It's great; however, you need to guarantee that your development process and team are mature enough to use it in the real world. In this workshop, we consider CD to mean **Continuous Delivery**.
{{% /notice %}}

According to the definition of CD, in order to reliably deploy changes to your software at any time, your codebase must have been thoroughly tested by the CI process. CI and CD are thus tightly coupled; you cannot safely deploy code changes at any time until your codebase is verified and works as expected.

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

The majority of deployment strategies have the common downsides of a lengthy rollback and launching the entire deployment before realizing anything is wrong. There are a number of deployment strategies that reduce stress when software is released. [canary release](https://martinfowler.com/bliki/CanaryRelease.html) (or *canary deployment*) and [blue/green deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html?ref=dombat.co.uk) are popular ones. 

In **canary release**, one instance (called the *canary*) is updated with the new version of software, and a small percentage of traffic is directed to it.

![0002](/images/2/2/0002.svg?featherlight=false&width=42pc)

 If the *canary* is in good condition, and there are two possible ways that the deployment can go forward: either by moving all traffic to instances that are running the updated version or by progressively generating more *canary* instances and rerouting all traffic to them until the old instances are no longer receiving any traffic at all. The selling point of the strategy is that only a small percentage of selected users may experience the outage if the *canary* is unhealthy. In that case, the entire process can be stopped and all traffic is routed back to the original instances. It drastically lowers the *time to restore service* in [DORA metrics](https://www.atlassian.com/devops/frameworks/dora-metrics) since the traffic is routed to the original instances all at once. 

In my opinion, although *canary release* is supported by AWS CodeDeploy, it fails to demonstrate the real meaning and potential of the deployment strategy. *canary release* by AWS CodeDeploy allows all software users to have access to the application versions proportionally, rather than just a subset of software users. The real power of *canary release* comes from the ability to select which users will receive the new version.

**Blue/Green Deployment** is another deployment strategy supported by AWS CodeDeploy.


#### Business Process Model and Notation (BPMN) 2.0

[BPMN](https://www.omg.org/spec/BPMN#document-metadata) - has become the de-facto standard for business processes diagrams (you should be familiar with it if you are a Business Analyst). It is meant for usage directly by stakeholders who create, manage, and implement business processes. A pipeline design defines the orchestration of a process or workflow and has many analogies to modeling business processes. The business process modeling paradigm can thus serve as the basis for pipeline design. BPMN diagrams used to build workflows or CI/CD pipelines provide insight into software delivery processes, such as stages, activities, and relationships with other systems. 

The notation used in this workshop is BPMN 2.0. BPMN 2.0 has a unique notation with identifiable icons known as *elements*. The table below shows a subset of the most used BPMN 2.0 elements that might be enough for designing your pipelines.

![0003](/images/2/2/0003.svg?featherlight=false&width=40pc)

#### The Proposal Workflow and Pipelines

Since AWS CodePipeline does not support stage execution in parallel


In the next section, you learn how to improve the pipelines

#### AWS Architecture