---
title : "Deployment Strategies"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

Even with the best tools and a code coverage of 100%, a bug-free *trunk* is not guaranteed by the CI pipeline; rather, it simply raises your confidence that the main codebase is in a releasable state. Without a suitable deployment strategy, even with a CI pipeline acting as a quality gate before merging back the PRs to the *trunk*, it could still be unsafe to deliver your software continually. If you want to release software more often without losing reliability, you may require a deployment strategy that reduces the impact of a production outage.

The AWSDSC-XUT team has faced challenges with prolonged production failures during deployments, affecting all users. Although these projects are non-profit and primarily serve students who may be more forgiving of occasional interruptions, the AWSome Books team is committed to building trust and minimizing risks by adopting a more reliable deployment strategy. This shift aims to provide a smoother experience and foster greater excitement and engagement among their college peers for using AWSome Books.

A common drawback of most deployment strategies is a lengthy rollback. There are a number of deployment strategies that reduce stress when software is released. *canary release* (or *canary deployment*) and *blue/green deployment* (or *red-black deployment*) are popular ones. 

#### Canary Release

In [canary release](https://martinfowler.com/bliki/CanaryRelease.html), one instance (called the *canary*) is updated with the new version of software, and a small percentage of traffic is directed to it.

 If the *canary* is in good condition, and there are two possible ways that the deployment can go forward: either by moving all traffic to instances that are running the updated version or by progressively generating more *canary* instances and rerouting all traffic to them until the old instances are no longer receiving any traffic at all. The selling point of the strategy is that only a small percentage of selected users may experience the outage if the *canary* is unhealthy. In that case, the entire process can be stopped and all traffic is routed back to the original instances. 

![0002](/images/2/2/0002.svg?featherlight=false&width=42pc)

In my opinion, although *canary release* is supported by AWS CodeDeploy, it fails to demonstrate the real meaning and potential of the deployment strategy. *canary release* by AWS CodeDeploy allows all software users to have access to the application versions proportionally, rather than just a subset of software users. The real power of *canary release* comes from the ability to select which users will receive the new version.

#### Blue/Green Deployment

[blue/green deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html?ref=dombat.co.uk) is another deployment strategy supported by AWS CodeDeploy. With *blue/green deployment*, you build a completely new set of instances and update them. Only after those instances are ready do you move traffic from the old (the "blue" instances, but it is not really important which ones are which color) to the new (the "green" instances).

*blue/green deployment* requires enough resources to run two full sets of instances at the same time. If you own and manage your own hardware, this might not be feasible unless you have a significant amount of extra capacity on hand. However, with AWS, this approach becomes much simpler and more cost-effective — you only pay for the extra resources during the deployment period.

![0003](/images/2/2/0004.svg?featherlight=false&width=42pc)

---

It drastically lowers the *failed deployment recovery time* in DORA metrics since both deployment strategies significantly reduce application downtime during deployments and rollbacks by rerouting traffic between old and new versions. However, the main difference lies in the potential impact during an outage. With *blue/green deployment*, all users might experience downtime if an issue arises. In contrast, *canary release* restricts the impact to only a small subset of users.

For deploying AWSome Books, you are going to use the *blue/green deployment* strategy, as implementing *canary release* can require substantial effort in a real-world scenario. AWS CodeDeploy with *blue/green deployment* streamlines the deployment process by offering a ready-made, production-grade solution, saving you the time and complexity of building one from scratch.
