---
title : "GitHub Actions Notifications To Slack"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 15.2 </b> "
---

**1.** Navigate to your Slack Workspace created in [4.2 Create Slack Channels](4-preparation/2-create-slack-channels).

- Expand the **Apps** dropdown.
- Click **Add apps**.

![0001](/images/15/2/0001.svg?featherlight=false&width=100pc)

**2.** Filter with value `github`, click **Add** for **GitHub**.

![0002](/images/15/2/0002.svg?featherlight=false&width=100pc)

**3.** Click **Add to Slack**.

![0003](/images/15/2/0003.svg?featherlight=false&width=100pc)

**4.** Click **Allow**.

![0004](/images/15/2/0004.svg?featherlight=false&width=100pc)

**5.** In your Slack Workspace,

- Select the **GitHub** app.
- Click **Connect GitHub account**.

![0005](/images/15/2/0005.svg?featherlight=false&width=100pc)

**6.** Click **Connect GitHub account**. 

![0006](/images/15/2/0006.svg?featherlight=false&width=100pc)

**7.** Copy the verification code.

![0007](/images/15/2/0007.svg?featherlight=false&width=100pc)

**8.** Back to the **GitHub** app in your Slack Workspace, click **Enter code**.

![0008](/images/15/2/0008.svg?featherlight=false&width=100pc)

**9.** Enter the code you have copied in the step **7** and click **Submit**.

![0009](/images/15/2/0009.svg?featherlight=false&width=100pc)

**10.** Enter `invite @GitHub` in the **gha-ci** channel and then click send icon.

![00010](/images/15/2/00010.svg?featherlight=false&width=100pc)

**11.** Click **Invite them**.

![00011](/images/15/2/00011.svg?featherlight=false&width=100pc)

**12.** Enter `/github subscribe fcj-workshops-2024/awsome-books` and then click send icon.

![00012](/images/15/2/00012.svg?featherlight=false&width=100pc)

**13.** Click **Install GitHub App**.

![00013](/images/15/2/00013.svg?featherlight=false&width=100pc)

**14.** Select **fcj-workshops-2024** GitHub organization.

![00014](/images/15/2/00014.svg?featherlight=false&width=100pc)

**15.**

- Select **Only select repositories**.
- For **Select repositories**, filter with value `awsome-books`.
- Select **fcj-workshops-2024/awsome-books**.

![00015](/images/15/2/00015.svg?featherlight=false&width=100pc)

**16.** Scroll down to the bottom, click **Install**.

![00016](/images/15/2/00016.svg?featherlight=false&width=100pc)

**17.** Enter `/github subscribe list` and then click send icon.

![00017](/images/15/2/00017.svg?featherlight=false&width=100pc)

**18.** Your **gha-ci** Slack channel should now be subscribed to **fcj-workshops-2024/awsome-books** repository.

![00017](/images/15/2/00018.svg?featherlight=false&width=100pc)

**19.** Apply the same steps (**10** to **18**) to the **gha-merge-group**, **gha-release**, and **gha-rollback** channels, but you can skip steps **13** through **16** for these.