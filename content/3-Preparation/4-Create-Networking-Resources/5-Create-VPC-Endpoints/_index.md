---
title : "Create VPC Endpoints"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 3.4.5 </b> "
---

#### Create ecr.dkr Interface Endpoint

**1.** Go to [AWS VPC console](https://console.aws.amazon.com/vpc/).

**2.** In the left sidebar,
- Choose **Endpoints**.
- Click **Create endpoint**.

![0001](/images/3/4/5/0001.svg?featherlight=false&width=100pc)

**3.** In the **Endpoint settings** section,
- For **Name tag**, enter `ecr-dkr-endpoint`.
- For **Service category**, select **AWS services**.
  
![0002](/images/3/4/5/0002.svg?featherlight=false&width=100pc)

In the **Services** section,
- Filter with `ecr.dkr` value.
- Choose **com.amazonaws.us-east-1.ecr.dkr**.

![0003](/images/3/4/5/0003.svg?featherlight=false&width=100pc)

In the **VPC** section, select the VPC named `fcj`.

![0004](/images/3/4/5/0004.svg?featherlight=false&width=100pc)

In the **Subnets** section,
- Select **us-east-1a** for **Availability Zone** and **subnet-id / fcj-private-02** for **Subnet ID**.
- Select **us-east-1b** for **Availability Zone** and **subnet-id / fcj-private-05** for **Subnet ID**.

![0005](/images/3/4/5/0005.svg?featherlight=false&width=100pc)

In the **Security groups** section, select **fcj-endpoint** security group.

![0006](/images/3/4/5/0006.svg?featherlight=false&width=100pc)

In the **Policy** section,
- Select **custom**.
- Fill out the following policy. Replace **\<YOUR-AWS-ACCOUNT-ID\>** with yours.

```
{
	"Statement": [
		{
			"Sid": "PreventDelete",
			"Effect": "Deny",
			"Principal": "*",
			"Action": "ecr:DeleteRepository",
			"Resource": "arn:aws:ecr:us-east-1:<YOUR-AWS-ACCOUNT-ID>:repository/awsome-books"
		},
		{
			"Sid": "AllowPull",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::<YOUR-AWS-ACCOUNT-ID>:role/ecsTaskExecutionRole"
			},
			"Action": [
				"ecr:BatchGetImage",
				"ecr:GetDownloadUrlForLayer"
			],
			"Resource": "arn:aws:ecr:us-east-1:<YOUR-AWS-ACCOUNT-ID>:repository/awsome-books"
		},
		{
			"Sid": "GetAuthorizationToken",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::<YOUR-AWS-ACCOUNT-ID>:role/ecsTaskExecutionRole"
			},
			"Action": [
				"ecr:GetAuthorizationToken"
			],
			"Resource": "*"
		}
	]
}
```

![0007](/images/3/4/5/0007.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Create endpoint**.

![0008](/images/3/4/5/0008.svg?featherlight=false&width=100pc)

#### Create ecr.api Interface Endpoint

**1.** Go to [AWS VPC console](https://console.aws.amazon.com/vpc/).

**2.** In the left sidebar,
- Choose **Endpoints**.
- Click **Create endpoint**.

![0009](/images/3/4/5/0001.svg?featherlight=false&width=100pc)

**3.** In the **Endpoint settings** section,
- For **Name tag**, enter `ecr-api-endpoint`.
- For **Service category**, select **AWS services**.
  
![00010](/images/3/4/5/0009.svg?featherlight=false&width=100pc)

In the **Services** section,
- Filter with `ecr.api` value.
- Choose **com.amazonaws.us-east-1.ecr.api**.

![00011](/images/3/4/5/00010.svg?featherlight=false&width=100pc)

In the **VPC** section, select the VPC named `fcj`.

![00012](/images/3/4/5/0004.svg?featherlight=false&width=100pc)

In the **Subnets** section,
- Select **us-east-1a** for **Availability Zone** and **subnet-id / fcj-private-02** for **Subnet ID**.
- Select **us-east-1b** for **Availability Zone** and **subnet-id / fcj-private-05** for **Subnet ID**.

![00013](/images/3/4/5/0005.svg?featherlight=false&width=100pc)

In the **Security groups** section, select **fcj-endpoint** security group.

![00014](/images/3/4/5/0006.svg?featherlight=false&width=100pc)

In the **Policy** section,
- Select **custom**.
- Fill out the following policy. Replace **\<YOUR-AWS-ACCOUNT-ID\>** with yours.

```
{
	"Statement": [
		{
			"Sid": "PreventDelete",
			"Effect": "Deny",
			"Principal": "*",
			"Action": "ecr:DeleteRepository",
			"Resource": "arn:aws:ecr:us-east-1:<YOUR-AWS-ACCOUNT-ID>:repository/awsome-books"
		},
		{
			"Sid": "AllowPull",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::<YOUR-AWS-ACCOUNT-ID>:role/ecsTaskExecutionRole"
			},
			"Action": [
				"ecr:BatchGetImage",
				"ecr:GetDownloadUrlForLayer"
			],
			"Resource": "arn:aws:ecr:us-east-1:<YOUR-AWS-ACCOUNT-ID>:repository/awsome-books"
		},
		{
			"Sid": "GetAuthorizationToken",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::<YOUR-AWS-ACCOUNT-ID>:role/ecsTaskExecutionRole"
			},
			"Action": [
				"ecr:GetAuthorizationToken"
			],
			"Resource": "*"
		}
	]
}
```

![00015](/images/3/4/5/0007.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Create endpoint**.

![00016](/images/3/4/5/0008.svg?featherlight=false&width=100pc)

#### Create s3 Gateway Endpoint

**1.** Go to [AWS VPC console](https://console.aws.amazon.com/vpc/).

**2.** In the left sidebar,
- Choose **Endpoints**.
- Click **Create endpoint**.

![00017](/images/3/4/5/0001.svg?featherlight=false&width=100pc)

**3.** In the **Endpoint settings** section,
- For **Name tag**, enter `s3-endpoint`.
- For **Service category**, select **AWS services**.
  
![00018](/images/3/4/5/00011.svg?featherlight=false&width=100pc)

In the **Services** section,
- Filter with `gateway` type.
- Choose **com.amazonaws.us-east-1.s3**.

![00019](/images/3/4/5/00012.svg?featherlight=false&width=100pc)

In the **VPC** section, select the VPC named `fcj`.

![00020](/images/3/4/5/00013.svg?featherlight=false&width=100pc)

In the **Route tables** setion, choose the main route table.

![00021](/images/3/4/5/00014.svg?featherlight=false&width=100pc)

In the **Policy** section,
- Select **custom**.
- Fill out the following policy.

```
{
	"Version": "2008-10-17",
	"Statement": [
		{
			"Sid": "Access-to-specific-bucket-only",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::prod-us-east-1-starport-layer-bucket/*"
		}
	]
}
```

![00022](/images/3/4/5/00015.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Create endpoint**.

![00023](/images/3/4/5/00016.svg?featherlight=false&width=100pc)

#### Create secretsmanager Interface Endpoint

**1.** Go to [AWS VPC console](https://console.aws.amazon.com/vpc/).

**2.** In the left sidebar,
- Choose **Endpoints**.
- Click **Create endpoint**.

![00024](/images/3/4/5/0001.svg?featherlight=false&width=100pc)

**3.** In the **Endpoint settings** section,
- For **Name tag**, enter `secretsmanager-endpoint`.
- For **Service category**, select **AWS services**.
  
![00025](/images/3/4/5/00017.svg?featherlight=false&width=100pc)

In the **Services** section,
- Filter with `secretsmanager` value.
- Choose **com.amazonaws.us-east-1.secretsmanager**.

![00026](/images/3/4/5/00018.svg?featherlight=false&width=100pc)

In the **VPC** section, select the VPC named `fcj`.

![00027](/images/3/4/5/0004.svg?featherlight=false&width=100pc)

In the **Subnets** section,
- Select **us-east-1a** for **Availability Zone** and **subnet-id / fcj-private-02** for **Subnet ID**.
- Select **us-east-1b** for **Availability Zone** and **subnet-id / fcj-private-05** for **Subnet ID**.

![00028](/images/3/4/5/0005.svg?featherlight=false&width=100pc)

In the **Security groups** section, select **fcj-endpoint** security group.

![00029](/images/3/4/5/0006.svg?featherlight=false&width=100pc)

In the **Policy** section,
- Select **custom**.
- Fill out the following policy. Replace **\<YOUR-AWS-ACCOUNT-ID\>** with yours.

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::<YOUR-AWS-ACCOUNT-ID>:role/ecsTaskRole"
			},
			"Action": [
				"secretsmanager:GetSecretValue",
				"secretsmanager:DescribeSecret"
			],
			"Resource": "arn:aws:secretsmanager:us-east-1:<YOUR-AWS-ACCOUNT-ID>:secret:*"
		}
	]
}

```

![00030](/images/3/4/5/00019.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Create endpoint**.

![00031](/images/3/4/5/00020.svg?featherlight=false&width=100pc)

#### Create CloudWatch Logs Interface Endpoint

{{% notice tip %}}
CloudWatch Logs seem to be really essential for debugging your AWS services in many circumstances. For example, without enabling logging, it could take hours or even days to analyze AWS ECS tasks that failed to run properly.
{{% /notice %}}