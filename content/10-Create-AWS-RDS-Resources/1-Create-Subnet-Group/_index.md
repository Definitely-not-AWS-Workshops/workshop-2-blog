---
title : "Create Subnet Group"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 10.1 </b> "
---

**1.** Go to [AWS RDS console](https://console.aws.amazon.com/rds/).

**2.** In the left sidebar,

- Choose **Subnet groups**.
- Click **Create DB subnet group**.

![0001](/images/10/1/0001.svg?featherlight=false&width=100pc)

**3.** In the **Subnet group details** section,

- For **Name**, enter `awsome-books`.
- For **Description**, enter `awsome-books subnet group`.
- For **VPC**, choose the subnet named **fcj**.

![0002](/images/10/1/0002.svg?featherlight=false&width=100pc)

**4.** In the **Add subnets** section,

- For **Availability Zones**, select **us-east-1a** and **us-east-1b**.
- For **Subnets**, choose subnets with CIDR ranges **10.0.64.0/19** and **10.0.160.0/19**.
- Click **Create**.

![0003](/images/10/1/0003.svg?featherlight=false&width=100pc)
