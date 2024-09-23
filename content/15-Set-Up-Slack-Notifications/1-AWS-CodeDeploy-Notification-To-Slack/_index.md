---
title : "AWS CodeDeploy Notification To Slack"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 15.1 </b> "
---

**1.** Go to [AWS Chatbot console](https://console.aws.amazon.com/chatbot/).

**2.** In the **Configure a chat client** section,

- For **Chat client**, select **Slack**.
- Click **Configure client**.

![0001](/images/15/1/0001.svg?featherlight=false&width=100pc)

**3.** Click **Allow**.

![0002](/images/15/1/0002.svg?featherlight=false&width=100pc)

**4.** Click **Configure new channel**.

![0003](/images/15/1/0003.svg?featherlight=false&width=100pc)

**5.** In the **Configuration details** section, enter `fcj-slack` for **Configuration name**. Next, you might need go to Slack Workspace to get the required channel ID.

![0004](/images/15/1/0004.svg?featherlight=false&width=100pc)

**6.** Navigate to your Slack Workspace created in [4.2 Create Slack Channels](4-preparation/2-create-slack-channels).

- Right click **aws-codedeploy** channel.
- Select **View channel details**.

![0005](/images/15/1/0005.svg?featherlight=false&width=100pc)

**7.** In the opening modal,

- Choose the **About** tab.
- Copy the **Channel ID**.

![0006](/images/15/1/0006.svg?featherlight=false&width=100pc)

**8.** Return to your AWS Chatbot configuration console, in the **Slack channel** section.

- For **Channel type**, select **Private**.
- For **Channel ID**, enter the channel ID you got from step **7**.

![0007](/images/15/1/0007.svg?featherlight=false&width=100pc)

**9.** In the **Permissions** section,

- For **Role settings**, select **Channel role**.
- For **Channel role**, select **Create an IAM role using a template**.
- For **Role name**, enter `chatbot-role`.

![0008](/images/15/1/0008.svg?featherlight=false&width=100pc)

**10.** In the **Notifications - optional** section, select **US East - N. Virginia** for **Region 1**.

![0009](/images/15/1/0009.svg?featherlight=false&width=100pc)

**11.** Scroll down to the bottom, click **Configure**.

![00010](/images/15/1/00010.svg?featherlight=false&width=100pc)

**12.** Note down the chat configuration ARN.

![00011](/images/15/1/00011.svg?featherlight=false&width=100pc)

**13.** Go to [AWS CodeDeploy console](https://console.aws.amazon.com/codedeploy/).

**14.** In the left sidebar,

- Expand the **Deploy - CodeDeploy** dropdown.
- Select **Applications**.
- Click **AppECS-fcj-awsome-books**.

![00012](/images/7/2/00015.svg?featherlight=false&width=100pc)

**15.** Expand the **Notify** dropdown, click **Create notification rule**.

![00013](/images/15/1/00012.svg?featherlight=false&width=100pc)

**16.** In the **Notification rule settings** section,

- For **Notification name**, enter `slack-notification`.
- For **Detail type**, select **Full**.
  
![00014](/images/15/1/00013.svg?featherlight=false&width=100pc)

**17.** In the **Events that trigger notifications**, click **Select all**.

![00015](/images/15/1/00014.svg?featherlight=false&width=100pc)

**18.** In the **Targets** section,

- For **Choose target type**, select **AWS Chatbot (Slack)**.
- For **Choose target**, enter the chat configuration ARN you have noted down in step **12**.
- Click **Create**.

![00016](/images/15/1/00015.svg?featherlight=false&width=100pc)

**19.** Navigate to your Slack Workspace created in [4.2 Create Slack Channels](4-preparation/2-create-slack-channels).

- Expand the **Apps** dropdown.
- Click **Add apps**.

![00017](/images/15/1/00016.svg?featherlight=false&width=100pc)

**20.** Filter with value `aws`, click **Add** for **AWS Chatbot**.

![00018](/images/15/1/00017.svg?featherlight=false&width=100pc)

**21.** Click **Add to Slack**.

![00019](/images/15/1/00018.svg?featherlight=false&width=100pc)