---
title : "Yet another CI/CD pipeline for containerized API application to AWS!"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---

# Yet another CI/CD pipeline for containerized API application to AWS!

#### Overview

Join our enjoyable workshop designed to elevate your Continuous Integration / Continuous Delivery (CI/CD) practices! Here’s what you will dive into:

- experience the simplified *trunk-based development* branching strategy to reduce merge conflicts and make code reviews easier.
- craft and optimize *automation pipelines* using GitHub Actions, leveraging job parallelism, dependency caching, merge queue, and concurrency group techniques.
- implement *blue/green deployment* with AWS CodeDeploy to minimize application deployment downtime.

In addition, a simple ChatOps approach will be used to centralize the pipeline and deployment results to Slack channels.
  
And that’s just the beginning! This workshop will unlock even more AWS services to enhance your containerized application's reliability and performance. Don't miss out!

You might implement the following AWS architecture. Exciting, right?

![0001](/images/0/0001.svg?featherlight=false&width=100pc)

*If you need complete control over the view of the image, check [here](https://drive.google.com/file/d/1N3jMhLBQQzXQKa8JfW5R7RGvMHI2_wnb/view?usp=sharing).*

You might notice that the VPC is primarily private, with no public subnets or Internet Gateway configurations. In this workshop, you are going to discover how to securely access private resources from the outside and how these resources initiate connections with other AWS services!

{{% notice note %}}
I believe the word "DevOps" involves more than just toolings; it is about cultivating the right team culture and applying tool-agnostic techniques to truly speed up the development process. This workshop aims to show the core ideas of a DevOps-driven CI/CD pipeline, rather than mastering security scanning, testing, or CI/CD technologies (other components of DevOps, including monitoring and observability, will not be covered). Once you grasp the fundamentals of a CI/CD pipeline, you can choose your preferred tools for implementation!
{{% /notice %}}

This workshop is inspired by my own ideas and one of my favorite books, [continuous delivery](https://www.amazon.co.uk/Grokking-Continuous-Delivery-Christie-Wilson/dp/1617298255) by [Christie Wilson](https://www.linkedin.com/in/christieawilson/?originalSubdomain=ca). This book covers the foundations of continuous delivery and provides examples of how to use them in your project. Give it a read if you are interested in accelerating the software delivery process with any stack.

![0002](/images/0/0001.jpg?featherlight=false&width=18pc)

{{% notice note %}}
You should work more on setting up and configuring this workshop to fit your needs and your projects, as it was made for me to learn from and has not been used on any real-world projects yet.
{{% /notice %}}

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

- An AWS account with administrative access.
- A GitHub account.
- Familiarity with GitHub and GitHub Actions.
- Basic understanding of the AWS CLI and related services.
- Experience with Spring-based API development and Docker (helpful but not required).
- Basic knowledge of Shell and Git commands.

You do not need deep knowledge on any of these, and if needed, you can research them as you go!

#### Duration
The estimated duration for completing this workshop is **around 6 hours**.

#### Costs
The services used in this workshop charge per usage, although most of them have a free tier, so unless you used the free tier for other workloads, it will cost you **around $1.5** to run this workshop. Please look at the different services' pricing pages to see their cost.

{{% notice note %}}
To avoid incurring charges, move to the [8. Cleanup](./8-cleanup/) section after the workshop to clean up the resources provisioned.
{{% /notice %}}

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

{{% notice info %}}
While it may have some shortcomings, it is definitely built with love 💖. If you found it useful and were able to apply anything to your projects, do not hesitate to give it a star 🌟 [here](https://github.com/Definitely-not-AWS-Workshops/workshop-2-blog).
{{% /notice %}}

We welcome your suggestions or improvements! If you have any feedback while running this workshop, feel free to send us an email to [tu.lna07@gmail.com](mailto:tu.lna07@gmail.com).


