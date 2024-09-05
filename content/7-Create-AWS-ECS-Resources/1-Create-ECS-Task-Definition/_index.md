---
title : "Create ECS Task Definition"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 7.1 </b> "
---


Let's create an AWS ECS Task Definition - a JSON file that defines the configuration of a task in Amazon Elastic Container Service (ECS). It acts like a blueprint that tells ECS how to run a Docker container, including information about the container image, resources, environment variables, networking, logging, and IAM roles.

**1.** Go to [AWS ECS console](https://console.aws.amazon.com/ecs/).

**2.** In the left sidebar,

- Choose **Task definitions**.
- Click **Create new task definition**.

![0001](/images/6/1/0001.svg?featherlight=false&width=100pc)

**3.** In the **Task definition configuration** section, enter `awsome-books` for **Task definition family**.

![0002](/images/6/1/0002.svg?featherlight=false&width=100pc)

**4.** In the **Infrastructure requirements** section,

- For **Task role**, choose **ecsTaskRole**.
- For **Task execution role**, choose **Create new role**.

![0003](/images/6/1/0003.svg?featherlight=false&width=100pc)


**5.** In the **Container - 1** section, 

- For **Name**, enter `awsome-books`.
- For **Image URI**, choose one that you have noted down in step **7** in [4. Create AWS ECR](4-create-aws-ecr).
- For **Container port**, enter `8080`.

![0004](/images/6/1/0004.svg?featherlight=false&width=100pc)

**6**. Scroll down to the bottom, click **Create**.

![0005](/images/6/1/0005.svg?featherlight=false&width=100pc)



