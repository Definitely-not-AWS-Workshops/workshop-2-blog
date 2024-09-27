---
title : "Test AWS RDS Failover"
date : "`r Sys.Date()`"
weight : 17
chapter : false
pre : " <b> 17. </b> "
---

Let’s put your database servers to the test by initiating a reboot to simulate a failover scenario.

**1.** Go to [AWS RDS console](https://console.aws.amazon.com/rds/).

**2.** In the left sidebar,

- Click **Databases**.
- Select **awsome-books** database.
- Expand the **Actions** dropdown.
- Click **Reboot**.

![0001](/images/17/0001.svg?featherlight=false&width=100pc)

**3.**

- Enable **Reboot With Failover**.
- Click **Confirm**.

![0002](/images/17/0002.svg?featherlight=false&width=100pc)

You might notice that your primary database server is in **Rebooting** status. Its current AZ should be **us-east-1a**.

![0003](/images/17/0003.svg?featherlight=false&width=100pc)

**3.** During the database failover process, you access the AWS API Gateway endpoint you recorded in step **9** of section [9.2 Create AWS API Gateway](9-create-aws-api-gateway-resources/2-create-aws-api-gateway/) with the **books** resource, you may encounter an **Internal Server Error** notification. This indicates that your application is unable to connect to the database server.

![0004](/images/17/0004.svg?featherlight=false&width=100pc)

**4.** After a short wait, the database status should update to **Available** with the AZ where the primary database server placed in set to **us-east-1b**.

![0005](/images/17/0005.svg?featherlight=false&width=100pc)

**5.** You can now successfully access the **books** resource via the AWS API Gateway default endpoint.

![0006](/images/17/0006.svg?featherlight=false&width=100pc)

