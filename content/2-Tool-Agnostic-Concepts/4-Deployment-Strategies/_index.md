---
title : "Deployment Strategies"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

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
