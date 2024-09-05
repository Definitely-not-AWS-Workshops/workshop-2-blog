---
title : "Create ECS Task Role"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4.3.2 </b> "
---

**1.** Go to [AWS IAM console](https://console.aws.amazon.com/iam/).

**2.** In the left sidebar,
- Choose **Policies**.
- Click **Create policy**.

![0001](/images/4/3/2/0001.svg?featherlight=false&width=100pc)

**3.** In **Policy editor** section,
- Select **JSON** tab.
- Fill out the following policy. Replace **\<YOUR-AWS-ACCOUNT-ID\>** with yours.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:DescribeSecret",
                "secretsmanager:GetSecretValue"
            ],
            "Resource": "arn:aws:secretsmanager:us-east-1:<YOUR-AWS-ACCOUNT-ID>:secret:*"
        }
    ]
}

```

![0002](/images/4/3/2/0002.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Next**.

![0003](/images/4/3/2/0003.svg?featherlight=false&width=100pc)

**4.** In **Policy details** section, enter `ecsTaskRolePolicy` for **Policy name**.

![0004](/images/4/3/2/0004.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Create policy**.

![0005](/images/4/3/2/0005.svg?featherlight=false&width=100pc)

**5.** In the left sidebar,
- Choose **Roles**.
- Click **Create role**.

![0006](/images/4/3/1/0001.svg?featherlight=false&width=100pc)

**6.** In **Trusted entity type** section, choose **Custom trust policy**.

![0007](/images/4/3/2/0006.svg?featherlight=false&width=100pc)

In **Custom trust policy** section, fill out the following policy.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "ecs-tasks.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}

```

![0008](/images/4/3/2/0007.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Next**.

![0009](/images/4/3/2/0008.svg?featherlight=false&width=100pc)

**7.** In the **Permissions policies** section,
- Filter with the value `ecsTaskRolePolicy`.
- Select **ecsTaskRolePolicy**.
- Click **Next**.

![00010](/images/4/3/2/0009.svg?featherlight=false&width=100pc)

**8.** In **Role details** section, enter `ecsTaskRole` for **Role name**.

![00011](/images/4/3/2/00010.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Create role**.

![00012](/images/4/3/2/00011.svg?featherlight=false&width=100pc)