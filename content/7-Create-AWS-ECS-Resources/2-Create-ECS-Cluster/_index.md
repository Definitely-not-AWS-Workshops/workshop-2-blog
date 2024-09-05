---
title : "Create ECS Cluster"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 7.2 </b> "
---

You now create an AWS ECS Cluster that uses the task definition from the previous section. 

**1.** Go to [AWS ECS console](https://console.aws.amazon.com/ecs/).

**2.** In the left sidebar,

- Choose **Clusters**.
- Click **Create cluster**.

![0001](/images/7/2/0001.svg?featherlight=false&width=100pc)

**3.** In the **Cluster configuration** section, enter `fcj` for **Cluster name**.

![0002](/images/7/2/0002.svg?featherlight=false&width=100pc)

**4.** Scroll down to the bottom, click **Create**.

![0003](/images/7/2/0003.svg?featherlight=false&width=100pc)

You next create an ECS service that run the AWSome Books application at scale.

**5.** In the **Services** tab of your newly created `fcj` ECS cluster details page, click **Create**.

![0004](/images/7/2/0004.svg?featherlight=false&width=100pc)

**6.** In the **Environment** section, select **Launch type** for **Compute options**.

![0005](/images/7/2/0005.svg?featherlight=false&width=100pc)

**7.** In the **Deployment configuration** section,

- For **Family**, choose **awsome-books**.
- For **Service name**, enter `awsome-books`.

![0006](/images/7/2/0006.svg?featherlight=false&width=100pc)

- For **Desired tasks**, enter 2.
- For **Deployment type**, choose **Blue/green deployment (powered by AWS CodeDeploy)**.
- For **Service role for CodeDeploy**, choose **ecsCodeDeployRole**.

![0007](/images/7/2/0007.svg?featherlight=false&width=100pc)

**8.** In the **Networking** section,

- For **VPC**, choose the VPC named **fcj**.
- For **Subnets**, select subnets **fcj-private-02** and **fcj-private-05**.
- For **Security group**, select **fcj-ecs-fargate**.
- For **Public IP**, disable **Turned off**.

![0008](/images/7/2/0008.svg?featherlight=false&width=100pc)

**9.** In the **Load balancing** section,

- For **Load balancer type**, choose **Application Load Balancer**.
- For **Application Load Balancer**, select **Use an existing load balancer**.
- For **Load balancer**, choose `fcj-alb`.

![0009](/images/7/2/0009.svg?featherlight=false&width=100pc)

- For **Target group 1 name**, enter `blue-tg-fcj-awsome-books`.
- For **Deregistration delay**, enter 10.

![00010](/images/7/2/00010.svg?featherlight=false&width=100pc)

Enter the same for target group 2, replace **Target group 2 name** as `green-tg-fcj-awsome-books` instead.

**10.** In the **Service auto scaling** section,

- Tick **Use service auto scaling**.
- For **Minimum number of tasks**, enter 2.
- For **Maximum number of tasks**, enter 4.
- For **Scaling policy type**, choose **Target tracking**.
- For **Policy name**, enter `target-tracking-policy`.
- For **ECS service metric**, select **ECSServiceAverageCPUUtilization**.

![00011](/images/7/2/00011.svg?featherlight=false&width=100pc)

- For **Target value**, enter 70.
- For **Scale-out cooldown period**, enter 60.
- For **Scale-in cooldown period**, enter 60.

![00012](/images/7/2/00012.svg?featherlight=false&width=100pc)

**11.** Scroll down to the bottom, click **Create**.

![00013](/images/7/2/00013.svg?featherlight=false&width=100pc)

**12.** ECS tasks are going to pull container image from ECR repository through VPC endpoints. You should see 2 running ECS tasks in the **awsome-books** ECS service after a while. You cannot, however, access these tasks directly or via Application Load Balancer since they are all private in the VPC without Internet access configurations.

![00014](/images/7/2/00014.svg?featherlight=false&width=100pc)


