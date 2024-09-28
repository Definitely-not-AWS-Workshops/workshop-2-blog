---
title : "Your Second Rollback Workflow Execution"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 18.2 </b> "
---

Let's try to revert to the v0.0.0 version.

**1.** On your remote **awsome-books** repository,

- Select the **Actions** tab.
- Choose the **Rollback** workflow.
- Expand the **Run workflow** dropdown.
- Enter the value `v0.0.0`.
- Click **Run workflow**.

![0001](/images/18/2/0001.svg?featherlight=false&width=100pc)

**2.** The second Rollback workflow execution should be triggered.

![0002](/images/18/2/0002.svg?featherlight=false&width=100pc)

**3.** In your Slack Workspace, a notification has been sent to the **aws-codedeploy** channel, indicating that the CodeDeploy Deployment has been triggered. Click the **AWS CodeDeploy Notification** to access the AWS CodeDeploy Deployment console.

![0003](/images/18/2/0003.svg?featherlight=false&width=100pc)

**4.** In the CodeDeploy Deployment console, wait for the deployment of the replacement task set to complete and for traffic to be directed to the new task set.

![0004](/images/18/2/0004.svg?featherlight=false&width=100pc)

**5.** Access the AWSome Books project with the AWS API Gateway endpoint you have noted down in step **9** in [9.2 Create AWS API Gateway](9-create-aws-api-gateway-resources/2-create-aws-api-gateway) with the **books** resource. You should receive a **Not Found** response, as the v0.0.0 version only returns the private IP addresses of the ECS tasks.

![0005](/images/18/2/0005.svg?featherlight=false&width=100pc)

In the absence of the **books** resource, the response might be the private IP addresses of the ECS tasks.

![0006](/images/18/2/0006.svg?featherlight=false&width=100pc)

**6.** Click **Terminate original task set**.

![0007](/images/18/2/0007.svg?featherlight=false&width=100pc)

**7.** You should receive a success notification from AWS CodeDeploy in the **aws-codedeploy**  channel, delivered through AWS Chatbot.

![0008](/images/18/2/0008.svg?featherlight=false&width=100pc)

**8.** Additionally, the **gha-release** channel will likely receive a success notification for your Rollback workflow.

![0009](/images/18/2/0009.svg?featherlight=false&width=100pc)
