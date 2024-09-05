---
title : "Create ECS CodeDeploy Role"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3.3 </b> "
---

**1.** Go to [AWS IAM console](https://console.aws.amazon.com/iam/).

**2.** In the left sidebar,
- Choose **Roles**.
- Click **Create role**.

![0001](/images/4/3/1/0001.svg?featherlight=false&width=100pc)

**3.** In **Trusted entity type** section, choose **AWS service**.

![0002](/images/4/3/3/0002.svg?featherlight=false&width=100pc)

In the **Use case** section,
- For **Service or use case**, choose **CodeDeploy**.
- For **Choose a use case for the specified service**, choose **CodeDeploy - ECS**.
- Click **Next**.

![0003](/images/4/3/3/0003.svg?featherlight=false&width=100pc)

**4.** Click **Next**.

![0004](/images/4/3/3/0004.svg?featherlight=false&width=100pc)

**5.** In **Role details** section, enter `ecsCodeDeployRole` for **Role name**.

![0005](/images/4/3/3/0005.svg?featherlight=false&width=100pc)

**6.** Scroll down to the bottom. Click **Create role**.

![0006](/images/4/3/3/0006.svg?featherlight=false&width=100pc)
