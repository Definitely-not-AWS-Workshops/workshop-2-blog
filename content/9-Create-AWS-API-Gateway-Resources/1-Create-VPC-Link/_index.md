---
title : "Create VPC Link"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 9.1 </b> "
---

**1.** Go to [AWS API Gateway console](https://console.aws.amazon.com/apigateway/).

**2.** In the left sidebar,

- Choose **VPC link**.
- Click **Create**.

![0001](/images/9/1/0001.svg?featherlight=false&width=100pc)

**3.** In the **Choose a VPC link version** section, select **VPC link for REST APIs**.

![0002](/images/9/1/0002.svg?featherlight=false&width=100pc)

**4.** In the **VPC link details** section,

- For **Name**, enter `vpc-link-nlb`.
- For **Target NLB**, enter the ARN of Network Load Balancer you have noted down in step **12** in [8. Create Network Load Balancer](8-create-network-load-balancer).

![0003](/images/9/1/0003.svg?featherlight=false&width=100pc)

**5.** Scroll down to the bottom, click **Create**.

![0004](/images/9/1/0004.svg?featherlight=false&width=100pc)

**6.** It may take some time to create a VPC link. Make a note of the VPC Link ID when it has been created.

![0005](/images/9/1/0005.svg?featherlight=false&width=100pc)
