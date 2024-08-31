---
title : "Create Internet Gateway"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.4.3 </b> "
---

**1.** Go to [AWS VPC console](https://console.aws.amazon.com/vpc/).

**2.** In the left sidebar,

- Choose **Internet gateways**.
- Click **Create internet gateway**.

![0001](/images/3/4/3/0001.svg?featherlight=false&width=100pc)

**3.** In the **Internet gateway setting** section,
- For **Name tag**, enter `fcj`.
- Click **Create internet gateway**.
  
![0002](/images/3/4/3/0002.svg?featherlight=false&width=100pc)

**4.** Click the **Actions** dropdown and then choose **Attach to VPC**.

![0003](/images/3/4/3/0003.svg?featherlight=false&width=100pc)

**5.** In the **VPC** section,
- For **Available VPCs**, choose the VPC named `fcj`.
- Click **Attach internet gateway**.

![0004](/images/3/4/3/0004.svg?featherlight=false&width=100pc)