---
title : "Create Application Load Balancer"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

You will set up a separate AWS Application Load Balancer rather than creating one from the AWS ECS service, which will result in AWS ECS Fargate tasks and Application Load Balancer network interfaces in the same subnets and using the same security groups.

**1.** Go to [AWS EC2 console](https://console.aws.amazon.com/ec2/).

**2.** In the left sidebar,

- Choose **Load Balancers**.
- Click **Create load balancer** dropdown.
- Select **Create Application Load Balancer**.

![0001](/images/5/0001.svg?featherlight=false&width=100pc)

**3.** In the **Basic configuration** section,
- For **Load balancer name**, enter `fcj-alb`.
- For **Scheme**, choose **Internal**.

![0003](/images/5/0003.svg?featherlight=false&width=100pc)

**4.** In the **Network mapping** section,
- For **VPC**, choose VPC named `fcj`.
- For **Availability Zones**, select subnets **fcj-private-01** and **fcj-private-04**, corresponding to AZs **us-east-1a** and **us-east-1b**, respectively.

![0004](/images/5/0004.svg?featherlight=false&width=100pc)

**5.** In the **Security groups** section, select **fcj-alb**.

![0005](/images/5/0005.svg?featherlight=false&width=100pc)

**6.** In the **Listeners and routing** section, click **Create target group** to go to the **Create target group** console. You will go back this section later to continue the load balancer configuration.

![0006](/images/5/0006.svg?featherlight=false&width=100pc)


**7.**  In the **Basic configuration** section, 
- For **Choose a target type**, choose **IP addresses**.
- For **Target group name**, enter `fcj`.

![0007](/images/5/0007.svg?featherlight=false&width=100pc)

**8.** Scroll down to the bottom. Click **Next**.

![0008](/images/5/0008.svg?featherlight=false&width=100pc)

**9.** Scroll down to the bottom. Click **Create target group**.

![0009](/images/5/0009.svg?featherlight=false&width=100pc)

**10.** Back to the **Listeners and routing** section in step **6**, choose the target group named `fcj`.

![00010](/images/5/00010.svg?featherlight=false&width=100pc)

**11.** Scroll down to the bottom. Click **Create load balancer**.

![00011](/images/5/00011.svg?featherlight=false&width=100pc)

Since you are going to use listener and target group created from AWS ECS service for Blue / Green deployment, you next delete existing ones.

**12.** In the details console of your newly created load balancer, scroll down to the bottom,
- Choose **Listeners and rules** tab.
- Select the listener with port **80** and forward to target group **fcj** settings.
- Click the **Manage listener** dropdown.
- Choose **Delete listener**.
  
![00012](/images/5/00012.svg?featherlight=false&width=100pc)

**13.** Enter **confirm** and then click **Delete**.

![00013](/images/5/00013.svg?featherlight=false&width=100pc)

**14.** Go to [AWS EC2 console](https://console.aws.amazon.com/ec2/).

**15.** In the left sidebar,

- Choose **Target Groups**.
- Choose the **fcj** target group.
- Click the **Actions** dropdown.
- Click **Delete**.

![00014](/images/5/00014.svg?featherlight=false&width=100pc)

**16.** Click **Yes, delete**.

![00015](/images/5/00015.svg?featherlight=false&width=100pc)

