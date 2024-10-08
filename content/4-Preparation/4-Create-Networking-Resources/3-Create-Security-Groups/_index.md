---
title : "Create Security Groups"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.4.3 </b> "
---

Since security groups' inbound and outbound rules are interdependent, you must first create them all with default settings and adjust the rules later.

**1.** Go to [AWS VPC console](https://console.aws.amazon.com/vpc/).

**2.** In the left sidebar,

- Choose **Security groups**.
- Click **Create security group**.

![0001](/images/4/4/3/0001.svg?featherlight=false&width=100pc)

**3.** In the **Basic details** section,
- For **Security group name**, enter `fcj-nlb`.
- For **Description**, enter `fcj-nlb`.
- For **VPC**, choose the VPC named `fcj`.
  
![0002](/images/4/4/3/0002.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Create security group**.

![0003](/images/4/4/3/0003.svg?featherlight=false&width=100pc)

Do the same to add the other security groups. Replace the value of each field using the following tables.

- For **fcj-alb** security group. 

| Field   |      Value      |
|----------|-------------|
| Security group name |  `fcj-alb` |
| Description |    `fcj-alb`   |
| VPC | `fcj` |

- For **fcj-ecs-fargate** security group. 

| Field   |      Value      |
|----------|-------------|
| Security group name |  `fcj-ecs-fargate` |
| Description |    `fcj-ecs-fargate`   |
| VPC | `fcj` |

- For **fcj-db** security group. 

| Field   |      Value      |
|----------|-------------|
| Security group name |  `fcj-db` |
| Description |    `fcj-db`   |
| VPC | `fcj` |

- For **fcj-vpc-endpoint** security group. 

| Field   |      Value      |
|----------|-------------|
| Security group name |  `fcj-vpc-endpoint` |
| Description |    `fcj-vpc-endpoint`   |
| VPC | `fcj` |

After completing, you got 5 security groups in total. You next modify their rules.

**4.** Choose `fcj-alb` security group. Select **Inbound rules** tab and then click **Edit inbound rules**.

![0004](/images/4/4/3/0004.svg?featherlight=false&width=100pc)

**5.** Follow the table, add inbound rule(s) and then click **Save rules**.

| #   |      Type      | Protocol | Port range | Source |
|----------|-------------|-------------|-------------|-------------|
| 1 |  `HTTP` | `TCP` | `80` | `Custom` - choose security group named `fcj-nlb` |

![0005](/images/4/4/3/0005.svg?featherlight=false&width=100pc)

**6.** Choose `fcj-alb` security group. Select **Outbound rules** tab and then click **Edit outbound rules**.

![0006](/images/4/4/3/0006.svg?featherlight=false&width=100pc)

**7.** Follow the table, add outbound rule(s) and then click **Save rules**.

| #   |      Type      | Protocol | Port range | Destination |
|----------|-------------|-------------|-------------|-------------|
| 1 |  `Custom TCP` | `TCP` | `8080` | `Custom` - choose security group named `fcj-ecs-fargate` |

![0007](/images/4/4/3/0007.svg?featherlight=false&width=100pc)

**8.** Do the same for the other security groups from step **4** to **7**.

- For `fcj-nlb` security group,

Outbound rule(s) 

| #   |      Type      | Protocol | Port range | Destination |
|----------|-------------|-------------|-------------|-------------|
| 1 |  `HTTP` | `TCP` | `80` | `Custom` - choose security group named `fcj-alb` |


- For `fcj-ecs-fargate` security group,

Inbound rule(s) 

| #   |      Type      | Protocol | Port range | Source |
|----------|-------------|-------------|-------------|-------------|
| 1 |  `Custom TCP` | `TCP` | `8080` | `Custom` - choose security group named `fcj-alb` |

Outbound rule(s) 

| #   |      Type      | Protocol | Port range | Destination |
|----------|-------------|-------------|-------------|-------------|
| 1 |  `PostgreSQL` | `TCP` | `5432` | `Custom` - choose security group named `fcj-db` |
| 2 |  `HTTPS` | `TCP` | `443` | `Custom` - choose security group named `fcj-vpc-endpoint` |
| 3 |  `HTTPS` | `TCP` | `443` | `Custom` - choose S3 prefix list named `com.amazonaws.us-east-1.s3 \| pl-63a5400a` |

- For `fcj-db` security group,

Inbound rule(s) 

| #   |      Type      | Protocol | Port range | Source |
|----------|-------------|-------------|-------------|-------------|
| 1 |  `PostgreSQL` | `TCP` | `5432` | `Custom` - choose security group named `fcj-ecs-fargate` |

- For `fcj-vpc-endpoint` security group,

Inbound rule(s) 

| #   |      Type      | Protocol | Port range | Source |
|----------|-------------|-------------|-------------|-------------|
| 1 |  `HTTPS` | `TCP` | `443` | `Custom` - choose security group named `fcj-ecs-fargate` |

Outbound rule(s) 

| #   |      Type      | Protocol | Port range | Destination |
|----------|-------------|-------------|-------------|-------------|
| 1 |  `HTTPS` | `TCP` | `443` | `Anywhere-IPv4` - `0.0.0.0/0` |



