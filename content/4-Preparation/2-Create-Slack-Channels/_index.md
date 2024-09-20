---
title : "Create Slack Channels"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

In this section, you are going to create [Slack channels](https://slack.com/help/articles/360017938993-What-is-a-channel) for centralizing notifications from AWS CodeDeploy and GitHub Actions workflow.

{{% notice info %}}
The Slack setup in the workshop does not fully align with Slack's best practices. For more details, please refer to the [Slack Docs](https://slack.com/help). 
{{% /notice %}}

Let’s start by creating a Slack workspace that represents your company or team.

**1.** Go to Slack [get started](https://slack.com/get-started#/createnew) page. Sign in with Google or another method of your choice.

![0001](/images/4/2/0001.svg?featherlight=false&width=100pc)

**2.** After successfully logging in, you might be asked to create a workspace. Click **Create a Workspace**.

![0002](/images/4/2/0002.svg?featherlight=false&width=100pc)

**3.** At step 1 of 5,

- For **What’s the name of your company or team?**, enter `fcj`.
- Click **Next**.

![0003](/images/4/2/0003.svg?featherlight=false&width=100pc)

**4.** At step 2 of 5,

- For **What’s your name?,** enter `fcj-member`.
- Click **Next**.

![0004](/images/4/2/0004.svg?featherlight=false&width=100pc)

**5.** At step 3 of 5, click **Skip this step**.

![0005](/images/4/2/0005.svg?featherlight=false&width=100pc)

Click **Skip Step**.

![0006](/images/4/2/0006.svg?featherlight=false&width=100pc)

**6.** At step 4 of 5,

- For **What’s your team working on right now?**, enter `devops on aws`.
- Click **Next**.


![0007](/images/4/2/0007.svg?featherlight=false&width=100pc)

**7.** At step 5 of 5, choose **Start with Free**.

![0008](/images/4/2/0008.svg?featherlight=false&width=100pc)

**8.** In the sidebar,

- Click **Add channels**.
- Choose **Create a new channel**.

![0009](/images/4/2/0009.svg?featherlight=false&width=100pc)

**9.** Enter **Name** as `aws-codedeploy` and then click **Next**.

![00010](/images/4/2/00010.svg?featherlight=false&width=100pc)

**10.** Select **Private — only specific people** option and then click **Create**.

{{% notice tip %}}
**Public** channels are great for sharing information that should be accessible to everyone in organization, while **private** channels are best for conversations that should be limited to specific people. If you only want those working on a particular project to have access, opt for a **private** channel.
{{% /notice %}}

![00011](/images/4/2/00011.svg?featherlight=false&width=100pc)

**11.** Click **Skip for now**.

![00012](/images/4/2/00012.svg?featherlight=false&width=100pc)

**12.** Do the same from step **8** to **11** to create the following private channels
- `gha-ci` for CI notifications from GitHub Actions, 
- `gha-merge-group` for *merge group* notifications from GitHub Actions, 
- `gha-release` for release notifications from GitHub Actions, and 
- `gha-rollback` for rollback notifications from GitHub Actions
 
You finally got 5 private Slack channels created.

![00013](/images/4/2/00013.svg?featherlight=false&width=15pc)