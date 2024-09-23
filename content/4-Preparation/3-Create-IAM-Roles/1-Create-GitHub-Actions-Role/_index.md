---
title : "Create GitHub Actions Role"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.3.1 </b> "
---

**1.** Go to [AWS IAM console](https://console.aws.amazon.com/iam/).

**2.** In the left sidebar,
- Choose **Identity providers**.
- Click **Add provider**.

![0001](/images/4/3/1/0002.svg?featherlight=false&width=100pc)

**3.** In the **Configure provider** section,

- For **Provider type**, select **OpenID Connect**.
- For **Provider URL**, enter `https://token.actions.githubusercontent.com`.
- For **Audience**, enter `sts.amazonaws.com`.

![0002](/images/4/3/1/0003.svg?featherlight=false&width=100pc)

**4.** Scroll down to the bottom, click **Add provider**.

![0003](/images/4/3/1/0004.svg?featherlight=false&width=100pc)

**5.** Back to [AWS IAM console](https://console.aws.amazon.com/iam/).

**6.** In the left sidebar,
- Choose **Policies**.
- Click **Create policy**.

![0004](/images/4/3/1/0005.svg?featherlight=false&width=100pc)

**7.** In the **Policy editor** section,

- Select **JSON** tab.
- Fill out the following policy. Replace **\<YOUR-AWS-ACCOUNT-ID\>** with yours.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowPushPull",
            "Effect": "Allow",
            "Action": [
                "ecr:BatchGetImage",
                "ecr:BatchCheckLayerAvailability",
                "ecr:CompleteLayerUpload",
                "ecr:GetDownloadUrlForLayer",
                "ecr:InitiateLayerUpload",
                "ecr:PutImage",
                "ecr:UploadLayerPart",
                "ecr:DescribeImages"
            ],
            "Resource": "arn:aws:ecr:us-east-1:<YOUR-AWS-ACCOUNT-ID>:repository/awsome-books"
        },
        {
            "Sid": "GetAuthorizationToken",
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken"
            ],
            "Resource": "*"
        },
        {
            "Sid": "RegisterAndDescribeTaskDefinition",
            "Effect": "Allow",
            "Action": [
                "ecs:RegisterTaskDefinition",
                "ecs:DescribeTaskDefinition"
            ],
            "Resource": "*"
        },
        {
            "Sid": "PassRolesInTaskDefinition",
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": [
                "arn:aws:iam::<YOUR-AWS-ACCOUNT-ID>:role/ecsTaskRole",
                "arn:aws:iam::<YOUR-AWS-ACCOUNT-ID>:role/ecsTaskExecutionRole"
            ]
        },
        {
            "Sid": "DeployService",
            "Effect": "Allow",
            "Action": [
                "ecs:UpdateService",
                "ecs:DescribeServices",
                "codedeploy:GetDeploymentGroup",
                "codedeploy:CreateDeployment",
                "codedeploy:GetDeployment",
                "codedeploy:GetDeploymentConfig",
                "codedeploy:RegisterApplicationRevision"
            ],
            "Resource": [
                "arn:aws:ecs:us-east-1:<YOUR-AWS-ACCOUNT-ID>:service/fcj/awsome-books",
                "arn:aws:codedeploy:us-east-1:<YOUR-AWS-ACCOUNT-ID>:deploymentgroup:AppECS-fcj-awsome-books/DgpECS-fcj-awsome-books",
                "arn:aws:codedeploy:us-east-1:<YOUR-AWS-ACCOUNT-ID>:deploymentconfig:*",
                "arn:aws:codedeploy:us-east-1:<YOUR-AWS-ACCOUNT-ID>:application:AppECS-fcj-awsome-books"
            ]
        }
    ]
}
```

![0005](/images/4/3/1/0006.svg?featherlight=false&width=100pc)

**8.** Scroll down to the bottom, click **Next**.

![0006](/images/4/3/1/0007.svg?featherlight=false&width=100pc)


**9.** In the **Policy details** section, enter `gha-policy` for **Policy name**.

![0007](/images/4/3/1/0008.svg?featherlight=false&width=100pc)

**10.** Scroll down to the bottom, click **Create policy**.

![0008](/images/4/3/1/0009.svg?featherlight=false&width=100pc)

**11.** Back to [AWS IAM console](https://console.aws.amazon.com/iam/).

**12.** In the left sidebar,
- Choose **Roles**.
- Click **Create role**.

![0009](/images/4/3/1/00010.svg?featherlight=false&width=100pc)

**13.** In the **Trusted entity type** section, select **Web identity**.

![00010](/images/4/3/1/00011.svg?featherlight=false&width=100pc)

**14.** In the **Web identity** section,

- For **Identity provider**, select **token.actions.githubusercontent.com**.
- For **Audience**, select **sts.amazonaws.com**.
- For **GitHub organization**, enter `fcj-workshops-2024`.
- For **GitHub repository**, enter `awsome-books`.
- Click **Next**.

![00011](/images/4/3/1/00012.svg?featherlight=false&width=100pc)

**15.** In the **Permissions policies** section,

- Filter with value `gha-policy`.
- Select **gha-policy** policy.
- Click **Next**.

![00012](/images/4/3/1/00013.svg?featherlight=false&width=100pc)

**16.** In the **Role details** section, enter `gha-role` for **Role name**.

![00013](/images/4/3/1/00014.svg?featherlight=false&width=100pc)

**17.** Scroll down to the bottom, click **Create role**.

![00014](/images/4/3/1/00015.svg?featherlight=false&width=100pc)

**18.** Note down the role ARN you just created — you may need it later!

![00015](/images/4/3/1/00016.svg?featherlight=false&width=100pc)