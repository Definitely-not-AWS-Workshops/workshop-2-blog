---
title : "Create Configuration Variables"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 11.1 </b> "
---

You are going to create variables to use across multiple workflows, defining them at the repository level.

{{% notice info %}}
Discover how to efficiently store and manage information in variables within GitHub Actions [here](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables).
{{% /notice %}}

**1.** Go to the repository that you have created in [4.1 Create AWSome Books Repository](4-preparation/1-create-awsome-books-repository). Click **Settings**.

![0001](/images/11/1/0001.svg?featherlight=false&width=100pc)

**2.** In the left sidebar, click **Secrets and variables** dropdown and then select **Actions**.

![0002](/images/11/1/0002.svg?featherlight=false&width=100pc)

**3.** Select **Variables** tab. In the **Repository variables** section, click **New repository variable**.

![0003](/images/11/1/0003.svg?featherlight=false&width=100pc)

**4.** In the **New variable** console,

- For **Name**, enter `AWS_REGION`.
- For **Value**, enter `us-east-1`.
- Click **Add variable**.

![0004](/images/11/1/0004.svg?featherlight=false&width=100pc)

**5.** Click **New repository variable** to add another variable.

![0005](/images/11/1/0005.svg?featherlight=false&width=100pc)

**6.** Repeat steps **4** and **5** for the other variables, using the information from the table below as a guide.

| Name  | Value  |
|---|---|
| `PROJECT`  | `awsome-books`  |
| `ROLE_TO_ASSUME`  | enter the role arn you have created in [4.3.1 Create GitHub Actions Role](4-preparation/3-create-iam-roles/1-create-github-actions-role)  |
| `JAVA_VERSION`  | `17`   |
| `JAVA_DISTRIBUTION`  | `temurin`   |
| `ECS_CLUSTER`  | `fcj` |
| `CODEDEPLOY_APPLICATION`  | `AppECS-fcj-awsome-books`   |
| `CODEDEPLOY_APPLICATION_GROUP`  | `DgpECS-fcj-awsome-books`   |


By the end, you should have a total of 8 variables configured at the repository level.

![0006](/images/11/1/0006.svg?featherlight=false&width=100pc)