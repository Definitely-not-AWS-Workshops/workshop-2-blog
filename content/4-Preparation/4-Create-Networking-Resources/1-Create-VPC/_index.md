---
title : "Create VPC"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.4.1 </b> "
---

**1.** Go to [AWS VPC console](https://console.aws.amazon.com/vpc/).

**2.** Click **Create VPC**.

![0001](/images/3/4/1/0001.svg?featherlight=false&width=100pc)

**3.** In the **VPC settings** section,

- For **Resources to create**, choose **VPC only**.
- For **Name tag**, enter `fcj`.
- For **IPv4 CIDR**, enter `10.0.0.0/16`.

![0002](/images/3/4/1/0002.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Create VPC**.

![0003](/images/3/4/1/0003.svg?featherlight=false&width=100pc)

**4.** Click the **Actions** dropdown and then choose **Edit VPC settings**.

![0004](/images/3/4/1/0004.svg?featherlight=false&width=100pc)

**5.** In the **DNS settings** section,

- Tick **Enable DNS hostnames**.
- Click **Save**.

![0005](/images/3/4/1/0005.svg?featherlight=false&width=100pc)