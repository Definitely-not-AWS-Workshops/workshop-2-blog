---
title : "The DevOps Research and Assessment (DORA) Metrics"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

You have probably heard that *"DevOps accelerates the software development process"* quite often, and that CI/CD involves *"continuous"*. But just how fast is fast? Are you delivering software in weeks or months, or could you be delivering in hours? You might not need to release your software in a matter of hours, but learning how to do it safely and efficiently? Now that sounds exciting, doesn’t it?

Organizations embracing DevOps often use various metrics to measure the success of their CI/CD pipelines, infrastructure automation, testing strategies, and overall development practices. The most popular ones are [DORA metrics](https://dora.dev/), which focus on evaluating software team performance through 4 key metrics ([2021 DORA report](https://dora.dev/research/2021/dora-report/)). These metrics are divided into 2 main categories: *throughput* and *stability*, providing a comprehensive view of both the speed and reliability of software delivery.

##### Throughput

Throughput measures the velocity of changes that are being made. DORA assesses throughput using the following metrics:

- **Change lead time** - this metric measures the time it takes for a code commit or change to be successfully deployed to production. It reflects the efficiency of your delivery pipeline.
- **Deployment frequency** - this metric measures how often application changes are deployed to production. Higher deployment frequency indicates a more agile and responsive delivery process.

##### Stability 

Stability measures the quality of the changes delivered and the team’s ability to repair failures. DORA assesses throughput using the following metrics:

- **Change fail percentage** - this metric measures the percentage of deployments that cause failures in production, requiring hotfixes or rollbacks. A lower change failure rate indicates a more reliable delivery process.
- **Failed deployment recovery time** - this metric measures the time it takes to recover from a failed deployment. A lower recovery time indicates a more resilient and responsive system.

As part of their evaluation, the DORA team also categorized software development teams into 4 performance tiers: *low*, *medium*, *high*, and *elite*. This ranking system offers a clear perspective on each team’s overall effectiveness and success .

|  Metric  |  Elite  | High  |  Medium  | Low  |
|---|---|---|---|---|
| **Deployment frequency** | multiple deploys per day | Between once per week and once per month  | Between once per month and once every 6 months  | Fewer than once per six months  |
| **Change lead time** | Less than one hour | Between one day and one week | Between one month and six months | More than six months |
| **Failed deployment recovery time** | Less than one hour | Less than one day | Between one day and one week | More than six months |
| **Change fail percentage** | 0%-15% | 16%-30% | 16%-30%| 16%-30% |

<!-- Even with the latest and greatest tools, you cannot truly accelerate your software development process without the right team culture — one where every team member should understand and follow the practices needed to increase the release rate.  -->

To achieve *high* or even *elite* performance in software delivery, it is crucial to adopt tool-agnostic methodologies and, more importantly, ensure that your team is equipped with the knowledge and skills to implement them effectively. 

