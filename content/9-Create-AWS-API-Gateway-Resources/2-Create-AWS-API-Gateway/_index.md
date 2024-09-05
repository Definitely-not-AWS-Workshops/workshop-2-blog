---
title : "Create AWS API Gateway"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 9.2 </b> "
---

**1.** Go to [AWS API Gateway console](https://console.aws.amazon.com/apigateway/).

**2.** In the left sidebar,

- Choose **APIs**.
- In the **REST API** section, click **Build**.

![0001](/images/8/2/0001.svg?featherlight=false&width=100pc)

**3.** In the **API details** section,

- Select **New API**.
- For **API name**, enter `awsome-books`.
- For **API endpoint type**, select **Regional**.
- Click **Create API**.

![0002](/images/8/2/0002.svg?featherlight=false&width=100pc)

**4.** Click **Create method**.

![0003](/images/8/2/0003.svg?featherlight=false&width=100pc)

**5.** In the **Method details** section,

- For **Method type**, select **ANY**.
- For **Integration type**, select **VPC link**.
- Turn on **VPC proxy integration**.

![0004](/images/8/2/0004.svg?featherlight=false&width=100pc)

- For **HTTP method**, select **ANY**.
- For **VPC link**, select **[Use stage variable]** and enter `${stageVariables.vpcLinkId}`. You define the vpcLinkId stage variable after deploying the API to a stage and set its value to the ID of the VpcLink.
- For **Endpoint URL**, enter the Network Load Balancer's DNS name you have noted down in step **12** in [7. Create Network Load Balancer](7-create-network-load-balancer).

![0005](/images/8/2/0005.svg?featherlight=false&width=100pc)

**6.** Scroll down to the bottom, click **Create method**.

![0006](/images/8/2/0006.svg?featherlight=false&width=100pc)

**7.** Click **Deploy API**.

![0007](/images/8/2/0007.svg?featherlight=false&width=100pc)

**8.** In the **Deploy API** modal,

- For **Stage**, select **\*New stage\***.
- For **Stage name**, enter `prod`.
- Click **Deploy**. 

![0008](/images/8/2/0008.svg?featherlight=false&width=30pc)

**9.** Under the **Stage details** section, note the resulting **Invoke URL**. You need it to invoke the API. Before doing that, you must set up the vpcLinkId stage variable.

![0009](/images/8/2/0009.svg?featherlight=false&width=100pc)

**10.** Scroll down to the bottom, in the **Stage variables** tab, click **Manage variables**.

![00010](/images/8/2/00010.svg?featherlight=false&width=100pc)

**11.** In the **Stage variables** section, click **Add stage variable**.

![00011](/images/8/2/00011.svg?featherlight=false&width=100pc)

- For **Name**, enter `vpcLinkId`.
- For **Value**, enter the VPC Link ID you have noted down in step **6** in [8.1 Create VPC Link](8-create-aws-api-gateway-resources/1-create-vpc-link).
- Click **Save**.

![00012](/images/8/2/00012.svg?featherlight=false&width=100pc)

**12.** To view the results, use the URL you obtained in step **9**. If you try to invoke more than once, you should see 2 internal IP addresses in 2 seperate subnets (Yours should be different from mine).

![00013](/images/8/2/00013.svg?featherlight=false&width=100pc)

![00014](/images/8/2/00014.svg?featherlight=false&width=100pc)

You can access the internal Network Load Balancer in a totally private VPC via AWS API Gateway with the VPC Link setting, which is incredible!