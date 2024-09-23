---
title : "Create AWS RDS Multi-AZ"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 10.2 </b> "
---

**1.** Go to [AWS RDS console](https://console.aws.amazon.com/rds/).

**2.** In the left sidebar,

- Choose **Databases**.
- Click **Create database**.

![0001](/images/10/2/0001.svg?featherlight=false&width=100pc)

**3.** In the **Choose a database creation method** section, select **Standard create**.

![0002](/images/10/2/0002.svg?featherlight=false&width=100pc)

**4.** In the **Engine options** section,

- For **Engine type**, select **PostgreSQL**.

![0003](/images/10/2/0003.svg?featherlight=false&width=100pc)

- For **Engine Version**, select **PostgreSQL 16.3-R2**.

![0004](/images/10/2/0004.svg?featherlight=false&width=100pc)

**5.** In the **Templates** section, select **Production**.

![0005](/images/10/2/0005.svg?featherlight=false&width=100pc)

**6.** In the **Availability and durability** section, select **Multi-AZ DB instance**.

![0006](/images/10/2/0006.svg?featherlight=false&width=100pc)

**7.** In the **Settings** section,

- For **DB instance identifier**, enter `awsome-books`.
- For **Master username**, enter `postgres`.
- For **Credentials management**, choose **Managed in AWS Secrets Manager - most secure**.

![0007](/images/10/2/0007.svg?featherlight=false&width=100pc)

**8.** In the **Instance configuration** section, select **Burstable classes (includes t classes)** and **db.t3.micro**.

![0008](/images/10/2/0008.svg?featherlight=false&width=100pc)

**9.** In the **Storage** section,

- For **Storage type**, select **General Purpose SSD (gp3)**.
- For **Allocated storage**, enter 20.

![0009](/images/10/2/0009.svg?featherlight=false&width=100pc)

**10.** In the **Connectivity** section,

- For **Virtual private cloud (VPC)**, select the VPC named **fcj**.

![00010](/images/10/2/00010.svg?featherlight=false&width=100pc)

- For **DB subnet group**, choose **awsome-books**.
- For **Public access**, select **No**.
- For **VPC security group (firewall)**, select **Choose existing**.
- For **Existing VPC security groups**, select **fcj-db**.

![00011](/images/10/2/00011.svg?featherlight=false&width=100pc)

**11.** In the **Monitoring** section, turn off **Turn on Performance Insights**.

![00012](/images/10/2/00012.svg?featherlight=false&width=100pc)

**12.** In the **Additional configuration** section,

- For **Initial database name**, enter `awsome_books`.

![00013](/images/10/2/00013.svg?featherlight=false&width=100pc)

- For **Deletion protection**, turn off **Enable deletion protection** (do not do this in real world projects).

![00014](/images/10/2/00014.svg?featherlight=false&width=100pc)

**13.** Scroll down to the bottom, click **Create database**.

![00015](/images/10/2/00015.svg?featherlight=false&width=100pc)

It migh take a while to create AWS RDS intances and the secret in AWS Secrets Manager.

**14.** Note down the **Endpoint** of the database server you have just created.

![00016](/images/10/2/00016.svg?featherlight=false&width=100pc)

**15.** Go to [AWS Secrets Manager console](https://console.aws.amazon.com/secretsmanager/).

**16.** Note down the **Secret name**, you might need it later!

![00017](/images/10/2/00017.svg?featherlight=false&width=100pc)









