---
title : "Clean Up"
date : "`r Sys.Date()`"
weight : 19
chapter : false
pre : " <b> 19. </b> "
---

Congratulations! You have done your lengthy workshop with running your pipelines and AWS services smoothly.  Let's clean up AWS resources to avoid additional charge.  

**1.** Go to [AWS API Gateway console](https://console.aws.amazon.com/apigateway/).

**2.** In the left sidebar,

- Choose **APIs**.
- Choose **awsome-books** APIs.
- Click **Delete**.

![0001](/images/19/0001.svg?featherlight=false&width=100pc)

**3.** 

- Enter `confirm`.
- Click **Delete**.

![0002](/images/19/0002.svg?featherlight=false&width=100pc)

**4.** In the left sidebar,

- Choose **VPC links**.
- Select **vpc-link-nlb**

![0003](/images/19/0003.svg?featherlight=false&width=100pc)

**5.** Click **Delete**.

![0004](/images/19/0004.svg?featherlight=false&width=100pc)

**6.** Go to [AWS ECS console](https://console.aws.amazon.com/ecs/).

**7.** In the left sidebar,

- Select **Clusters**.
- Click the **fcj** cluster.
  
![0005](/images/19/0005.svg?featherlight=false&width=100pc)

**8.** Click **Delete**.

![0006](/images/19/0006.svg?featherlight=false&width=100pc)

**9.**

- Enter `delete fcj`.
- Click **Delete**.

![0007](/images/19/0007.svg?featherlight=false&width=100pc)

**10.** Go to [AWS EC2 console](https://console.aws.amazon.com/ec2).

**11.** In the left sidebar,

- Select **Target Groups**.
- Choose **blue-tg-fcj-awsome-books** and **tg-alb** target groups.
- Expand **Actions**.
- Click **Delete**.

![0008](/images/19/0008.svg?featherlight=false&width=100pc)

**12.** Click **Yes, delete**.

![0009](/images/19/0009.svg?featherlight=false&width=100pc)

**13.** In the left sidebar,

- Select **Load Balancers**.
- Choose **fcj-nlb** and **fcj-alb** load balancers.
- Expand **Actions**.
- Click **Delete load balancer**.

![00010](/images/19/00010.svg?featherlight=false&width=100pc)

**14.**

- Enter `confirm`.
- Click **Delete**.

![00011](/images/19/00011.svg?featherlight=false&width=100pc)

**15.** Go to [AWS RDS console](https://console.aws.amazon.com/rds).

**16.** In the left sidebar,

- Select **Databases**.
- Choose **awsome-books** database.
- Expand **Actions**.
- Click **Delete**.

![00012](/images/19/00012.svg?featherlight=false&width=100pc)

**17.**

- Turn off **Create final snapshot**.
- Turn off **Retain automated backups**.
- Enable **I acknowledge that upon instance deleteion, automated backups, including system snapshots and point-in-time recovery, will no longer be available**.
- Enter `delete me`.
- Click **Delete**.

![00013](/images/19/00013.svg?featherlight=false&width=100pc)

**18.**

- Click **Subnet groups**.
- Select **awsome-books** subnet group.
- Click **Delete**.

![00014](/images/19/00014.svg?featherlight=false&width=100pc)

**19.** Click **Delete**.

![00015](/images/19/00015.svg?featherlight=false&width=100pc)

**20.** Go to [AWS VPC console](https://console.aws.amazon.com/vpc).

**21.** In the left sidebar,

- Select **Endpoints**.
- Select **ecr-dkr-endpoint**, **ecr-api-endpoint**, **s3-endpoint**, and **secretsmanager-endpoint**.
- Expand **Actions**.
- Click **Delete VPC endpoints**.

![00016](/images/19/00016.svg?featherlight=false&width=100pc)

**22.**

- Enter `delete`.
- Click **Delete**.

![00017](/images/19/00017.svg?featherlight=false&width=100pc)

**23.**

- Select **Your VPCs**.
- Select **fcj** VPC.
- Expand **Actions**.
- Click **Delete VPC**.

![00018](/images/19/00018.svg?featherlight=false&width=100pc)

**24.**

- Enter `delete`.
- Click **Delete**.

![00019](/images/19/00019.svg?featherlight=false&width=100pc)

**25.** Go to [AWS ECR console](https://console.aws.amazon.com/ecr).

**26.** In the left sidebar,

- Click **Repositoires** under **Private registry**.
- Select **awsome-books** repository.
- Click **Delete**.

![00020](/images/19/00020.svg?featherlight=false&width=100pc)

**27.** Click **Delete**.

![00021](/images/19/00021.svg?featherlight=false&width=100pc)

**28.** Go to [AWS Chatbot console](https://console.aws.amazon.com/chatbot).

**30.** In the left sidebar,

- Select **Configured clients**.
- Select **Slack - fcj**.

![00022](/images/19/00022.svg?featherlight=false&width=100pc)

**31.**

- Select **fcj-slack** channels.
- Click **Delete**.

![00023](/images/19/00023.svg?featherlight=false&width=100pc)

**32.** Click **Delete**.

![00024](/images/19/00024.svg?featherlight=false&width=100pc)

**34.** Click **Remove workspace configuration**.

![00025](/images/19/00025.svg?featherlight=false&width=100pc)

**35.**

- Enter your **Workspace ID**.
- Click **Remove workspace configuration**.

![00026](/images/19/00026.svg?featherlight=false&width=100pc)

