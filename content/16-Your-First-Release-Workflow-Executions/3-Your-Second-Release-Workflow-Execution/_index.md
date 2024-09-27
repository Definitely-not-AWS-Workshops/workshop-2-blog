---
title : "Your Second Release Workflow Execution"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 16.3 </b> "
---

When you run the Release process with the correct SemVer format, AWSome Books project should be successfully delivered to AWS. 

**1.** On your remote **awsome-books** repository, click **Create a new release**.

![0001](/images/16/2/0001.svg?featherlight=false&width=100pc)

**2.**

- Select the **Releases** tab.
- Expand the **Choose a tag** dropdown.
- Enter with value `v0.0.1`.
- Click **Create new tag: v0.0.1**.

![0002](/images/16/3/0001.svg?featherlight=false&width=100pc)

**3.** Scroll down to the bottom, click **Publish release**.

![0003](/images/16/2/0003.svg?featherlight=false&width=100pc)

**4.** A notification has been sent to the **gha-release** channel in your Slack Workspace, signaling that a Release workflow has been triggered.

![0004](/images/16/3/0002.svg?featherlight=false&width=100pc)

**5.** Go to [AWS ECR console](https://console.aws.amazon.com/ecr/).

**6.** In the left sidebar,

- Expand the **Private registry** dropdown.
- Click **Repositories**.
- Select **awsome-books** repository.

![0004](/images/16/3/0003.svg?featherlight=false&width=100pc)

**7.** After a short while, the GitHub Actions job responsible for publishing the image with the v0.0.1 tag to the AWS ECR repository will do it work.

![0005](/images/16/3/0004.svg?featherlight=false&width=100pc)

**8.** A notification has been sent to the **aws-codedeploy** channel in your Slack Workspace, signaling that a CodeDeploy deployment has been triggered.

![00015](/images/16/3/00017.svg?featherlight=false&width=100pc)

**9.** Go to [AWS CodeDeploy console](https://console.aws.amazon.com/codedeploy/).

**10.** In the left sidebar,

- Expand the **Deploy - CodeDeploy** dropdown.
- Select **Deployments**.
- Click the deployment ID that is in progress.

![00011](/images/16/3/0005.svg?featherlight=false&width=100pc)

**11.** You might notice that AWS CodeDeploy is deploying the replacement task set.

![00012](/images/16/3/0006.svg?featherlight=false&width=100pc)

**12.** Once the replacement task set has been deployed successfully, your traffic should to be directed to the newly created task set after some time. You may stop the current deployment and revert to the previous task set during that period, as your old task set should persist until the timeout has passed.

![00013](/images/16/3/0007.svg?featherlight=false&width=100pc)

**13.** Go to [AWS API Gateway console](https://console.aws.amazon.com/apigateway/).

**14.** In the left sidebar,

- Select **APIs**.
- Click **awsome-books**.

![00013](/images/16/3/0008.svg?featherlight=false&width=100pc)

**15.** Click **Create resource**.

![00013](/images/16/3/0009.svg?featherlight=false&width=100pc)

**16.** In the **Resource details** section,

- For **Resource path**, enter `/`.
- For **Resource name**, enter `books`.
- Click **Create resource**.

![00013](/images/16/3/00010.svg?featherlight=false&width=100pc)

**17.** Click **Create method**.

![00013](/images/16/3/00011.svg?featherlight=false&width=100pc)

**18.** In the **Method details** section,

- For **Method type**, select **ANY**.
- For **Integration type**, select **VPC link**.
- Turn on **VPC proxy integration**.

![00013](/images/9/2/0004.svg?featherlight=false&width=100pc)

- For **HTTP method**, select **ANY**.
- For **VPC link**, select **[Use stage variable]** and enter `${stageVariables.vpcLinkId}`. You define the vpcLinkId stage variable after deploying the API to a stage and set its value to the ID of the VpcLink.
- For **Endpoint URL**, enter the Network Load Balancer's DNS name you have noted down in step **12** in [8. Create Network Load Balancer](8-create-network-load-balancer) with the **books** resource.

![00013](/images/16/3/00012.svg?featherlight=false&width=100pc)

**19.** Scroll down to the bottom, click **Create method**.

![00013](/images/16/3/00013.svg?featherlight=false&width=100pc)

**20.** Click **Deploy API**.

![00013](/images/16/3/00014.svg?featherlight=false&width=100pc)

**21.** In the opening modal,

- For **Stage**, select **prod**.
- Click **Deploy**.

![00013](/images/16/3/00015.svg?featherlight=false&width=30pc)

**22.** Access the AWSome Books project with the AWS API Gateway endpoint you have noted down in step **9** in [9.2 Create AWS API Gateway](9-create-aws-api-gateway-resources/2-create-aws-api-gateway).

![00017](/images/16/3/00022.svg?featherlight=false&width=100pc)

Also, the endpoint with the **books** resource.

![00014](/images/16/3/00016.svg?featherlight=false&width=100pc)

**23.** Back to your CodeDeploy Deployment console, click **Terminate original task set**.

![00016](/images/16/3/00018.svg?featherlight=false&width=100pc)

Once AWS CodeDeploy completes the termination of the original task set, you can move over to your Slack Workspace.

![00017](/images/16/3/00019.svg?featherlight=false&width=100pc)

**24.** You should receive a success notification from AWS CodeDeploy in the **aws-codedeploy** channel, delivered through AWS Chatbot.

![00017](/images/16/3/00020.svg?featherlight=false&width=100pc)

**25.** Additionally, the **gha-release** channel will likely receive a success notification for your Release workflow.

![00017](/images/16/3/00021.svg?featherlight=false&width=100pc)



