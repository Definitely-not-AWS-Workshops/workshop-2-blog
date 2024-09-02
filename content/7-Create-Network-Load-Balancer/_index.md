---
title : "Create Network Load Balancer"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

You next create a Network Load Balancer that passes through the unencrypted traffic from the VPC Link to Aplication Load Balancer.

**1.** Go to [AWS EC2 console](https://console.aws.amazon.com/ec2/).

**2.** In the left sidebar,

- Choose **Load Balancers**.
- Click **Create load balancer** dropdown.
- Select **Create Network Load Balancer**.

![0001](/images/7/0001.svg?featherlight=false&width=100pc)

**3.** In the **Basic configuration** section,
- For **Load balancer name**, enter `fcj-nlb`.
- For **Scheme**, choose **Internal**.

![0002](/images/7/0002.svg?featherlight=false&width=100pc)

**4.** In the **Network mapping** section,
- For **VPC**, choose VPC named `fcj`.
- For **Availability Zones**, select subnets **fcj-private-01** and **fcj-private-04**, corresponding to AZs **us-east-1a** and **us-east-1b**, respectively.

![0003](/images/7/0003.svg?featherlight=false&width=100pc)

**5.** In the **Security groups** section, select **fcj-nlb**.

![0004](/images/7/0004.svg?featherlight=false&width=100pc)

**6.** In the **Listeners and routing** section, click **Create target group** to go to the **Create target group** console. You will go back this section later to continue the load balancer configuration.

![0005](/images/7/0005.svg?featherlight=false&width=100pc)

**7.**  In the **Basic configuration** section, 
- For **Choose a target type**, choose **Application Load Balancer**.
- For **Target group name**, enter `tg-alb`.

![0006](/images/7/0006.svg?featherlight=false&width=100pc)

**8.** Scroll down to the bottom, click **Next**.

![0007](/images/7/0007.svg?featherlight=false&width=100pc)

**9.** In the **Register Application Load Balancer** section,

- For **Application Load Balancer**, enter `fcj-alb`.
- Click **Create target group**.

![0008](/images/7/0008.svg?featherlight=false&width=100pc)

**10.** Back to the **Listeners and routing** section in step **6**, choose the target group named `tg-alb`.

![0009](/images/7/0009.svg?featherlight=false&width=100pc)

**11.** Scroll down to the bottom, click **Create load balancer**.

![00010](/images/7/00010.svg?featherlight=false&width=100pc)

**12.** Note down the DNS name of Network Load balancer for later use.

![00011](/images/7/00011.svg?featherlight=false&width=100pc)

**13.** Scroll down to the bottom,

- Select **Security** tab.
- Click **Edit**.

![00012](/images/7/00012.svg?featherlight=false&width=100pc)

**14.** In the **Security setting** section,

- Turn off **Enforce inbound rules on PrivateLink traffic**.
- Click **Save changes**.

![00013](/images/7/00013.svg?featherlight=false&width=100pc)

The Network Load Balancer is internal and cannot be accessed publicly. You might need an AWS API Gateway with VPC Link configuration to connect to the Network Load Balancer.