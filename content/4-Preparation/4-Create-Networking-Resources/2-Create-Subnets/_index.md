---
title : "Create Subnets"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.4.2 </b> "
---

**1.** Go to [AWS VPC console](https://console.aws.amazon.com/vpc/).

**2.** In the left sidebar,

- Choose **Subnets**.
- Click **Create subnet**.

![0001](/images/4/4/2/0001.svg?featherlight=false&width=100pc)

**3.** In the **VPC** section, choose the VPC named `fcj` for **VPC ID**.

![0002](/images/4/4/2/0002.svg?featherlight=false&width=100pc)

**4.** In the **Subnet settings** section,

- For **Subnet name**, enter `fcj-private-01`.
- For **Availability Zone**, choose `US East (N. Virginia) / us-east-1a`.
- For **IPv4 subnet CIDR block**, enter `10.0.0.0/19`.

![0003](/images/4/4/2/0003.svg?featherlight=false&width=100pc)

**5.** Click **Add new subnet**.

![0004-1](/images/4/4/2/0004.svg?featherlight=false&width=100pc)

**6.** Do the same from step **4** to **5** to add the other subnets. Replace the value of each field using the following tables.

- For **fcj-private-02** subnet. 

| Field   |      Value      |
|----------|-------------|
| Subnet name |  `fcj-private-02` |
| Availability Zone |    `US East (N. Virginia) / us-east-1a`   |
| IPv4 subnet CIDR block | `10.0.32.0/19` |

- For **fcj-private-03** subnet. 

| Field   |      Value      |
|----------|-------------|
| Subnet name |  `fcj-private-03` |
| Availability Zone |    `US East (N. Virginia) / us-east-1a`   |
| IPv4 subnet CIDR block | `10.0.64.0/19` |

- For **fcj-private-04** subnet. 

| Field   |      Value      |
|----------|-------------|
| Subnet name |  `fcj-private-04` |
| Availability Zone |    `US East (N. Virginia) / us-east-1b`   |
| IPv4 subnet CIDR block | `10.0.96.0/19` |

- For **fcj-private-05** subnet. 

| Field   |      Value      |
|----------|-------------|
| Subnet name |  `fcj-private-05` |
| Availability Zone |    `US East (N. Virginia) / us-east-1b`   |
| IPv4 subnet CIDR block | `10.0.128.0/19` |

- For **fcj-private-06** subnet. 

| Field   |      Value      |
|----------|-------------|
| Subnet name |  `fcj-private-06` |
| Availability Zone |    `US East (N. Virginia) / us-east-1b`   |
| IPv4 subnet CIDR block | `10.0.160.0/19` |
    
**7.** After completing, you got 6 private subnets. Click **Create subnet**.

![0004-2](/images/4/4/2/0004-2.svg?featherlight=false&width=100pc)

<!-- **4.** Choose **fcj-public-01** subnet. Click the **Actions** dropdown and choose **Edit subnet settings**.

![0005](/images/4/4/2/0005.svg?featherlight=false&width=100pc)

**5.** In the **Auto-assign IP settings** section, tick **Enable auto-assign public IPv4 address**.

![0006](/images/4/4/2/0006.svg?featherlight=false&width=100pc)

**6.** Do the same for **fcj-public-04** from step **4** to **5**. -->