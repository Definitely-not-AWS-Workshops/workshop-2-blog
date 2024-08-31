---
title : "Yet another CI/CD pipeline for containerized API application to AWS!"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---

# Yet another CI/CD pipeline for containerized API application to AWS!

#### Overview

Join our dynamic workshop designed to elevate your CI/CD practices! Here’s what you will dive into:

- Experience the simplified *trunk-based development* branching strategy to reduce merge conflicts and make code reviews easier.
- Craft and optimize *automation pipelines* using GitHub Actions, leveraging job parallelism, dependency caching, and concurrency group techniques.
- Implement *Blue/Green deployment* with AWS CodeDeploy to minimize application downtime.

In addition, a simple ChatOps approach will be used to centralize the pipeline and deployment results to Slack channels.
  
And that’s just the beginning! This workshop will unlock even more AWS services to enhance your containerized application's reliability and performance. Don't miss out!

You might implement the following AWS architecture. Exciting, right?

![0001](/images/0/0001.svg?featherlight=false&width=100pc)

<!-- Take a look at some of the operations you might be engaged in. Do not worry if you do not understand now, it will be clear later!

Local development to CI workflow triggers and update dependency cache (You will skip the majority of local development processes for the sake of simplicity in hands-on sections).

![0002](/images/0/0002.svg?featherlight=false&width=100pc)

The release process might be simple as

![0003](/images/0/0003.svg?featherlight=false&width=100pc)

You can also start the rollback process manually.

![0004](/images/0/0004.svg?featherlight=false&width=100pc) -->

#### Target Audience
This beginner-level workshop is designed primarily for individuals in technical roles, but anyone interested in learning how to implement a CI/CD pipeline for a containerized Java API application will find it beneficial.

To participate in this workshop, the following prerequisites are recommended:

- An AWS account with an administrative user.
- A GitHub account.
- Familiarity with AWS CLI and related services.
- Familiarity with Java-based API development and Docker (good to know).
- Basic knowledge of shell and Git commands.

#### Duration
The estimated duration for completing this workshop is **around 4 hours**.

#### Costs
Assume that you own a domain name. The services used in this workshop charge per usage, although most of them have a free tier, so unless you used the free tier for other workloads, it will cost you **under $0.5** to run this workshop. Please look at the different services' pricing pages to see their cost.

{{% notice note %}}
To avoid incurring charges, move to the [8. Cleanup](./8-cleanup/) section after the workshop to clean up the resources provisioned.
{{% /notice %}}

#### Github Repositories ####

Visit the following Github repositories to find the complete workshop source codes:

|  Repository |  Description |
|---|---|
| [workshop&#8209;1&#8209;web&#8209;app](https://github.com/Definitely-not-AWS-Workshops/workshop-1-web-app)  |  This repository contains the source code for both web and app components. You will deploy these components to AWS infrastructure|
|  [workshop&#8209;1&#8209;tf&#8209;modules](https://github.com/Definitely-not-AWS-Workshops/workshop-1-tf-modules) |  This repository defines reusable modules. Consider every module to be a *"blueprint"* that describes specific elements of your infrastructure  |
|  [workshop&#8209;1&#8209;tf&#8209;live](https://github.com/Definitely-not-AWS-Workshops/workshop-1-tf-live) | This repository defines the live infrastructure you are running in each environment (stage, prod, mgmt, etc.). Consider this as the *"houses"* you constructed using the *"blueprints"* found in the [workshop-1-tf-modules](https://github.com/Definitely-not-AWS-Workshops/workshop-1-tf-modules) |


#### Main Content

1. [Introduction](./1-introduction/)
2. [High-Level Design](./2-high-level-design/)
3. [Preparation](./3-preparation/)
4. [Terraform Modules Repository](./4-terraform-modules-repository/)
5. [Terraform Live Repository](./5-terraform-live-repository/)
6. [AnimeHub Deployment](./6-animehub-deployment/)
7. [Experiments](./7-experiments/)
8. [Cleanup](./8-cleanup/)
9. [Further Improvements](./9-further-improvements/)
<!-- need to remove parenthesis for path in Hugo 0.88.1 for Windows-->


We welcome your suggestions or improvements! If you have any feedback while running this workshop, feel free to send us an email to [tu.lna07@gmail.com](mailto:tu.lna07@gmail.com).