---
title : "AWS Architecture"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

While the AWSDSC-XUT team will focus on building advanced features, you are going to concentrate on optimizing the core infrastructure for seamless operations and scalability. With a minimal working version of the [AWSome Books](https://github.com/Definitely-not-AWS-Workshops/workshop-2-awsome-books) already in place, your goal is to recommend and deploy the essential AWS services needed to efficiently run a containerized RESTful API application at scale.

Before we dive into the AWSome Books AWS architecture, let's first examine the key AWS services that can effortlessly support a containerized RESTful API application.

#### AWS Services

**AWS RDS**

[AWS RDS](https://docs.aws.amazon.com/rds/) simplifies the setup, operation, and scaling of relational databases in the cloud, offering cost-efficient, resizable capacity while handling common administrative tasks.

![0001](/images/3/2/0001.svg?featherlight=false&width=4pc)

The AWSome Books project leverages PostgreSQL as its relational database, fully supported by Amazon RDS. By enabling [Multi-AZ instance deployments](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZSingleStandby.html), AWS RDS ensures high availability and automatic failover, offering robust support and resilience for your database with a standby instance in another availability zone.

**AWS Secrets Manager**

[AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/) securely encrypts, stores, and retrieves credentials for databases and services, eliminating the need to hardcode them in your applications. By calling AWS Secrets Manager when needed, you can enhance security, automate secret rotation, and manage access, protecting your IT resources and data effectively.

![0002](/images/3/2/0002.svg?featherlight=false&width=4pc)


With AWS RDS's built-in support for AWS Secrets Manager, you can keep your AWSome Books codebase clean and secure — no more hardcoding database credentials!

**AWS ECR**

[AWS ECR](https://docs.aws.amazon.com/ecr/) is a fully managed container registry that offers high-performance image hosting, allowing you to seamlessly deploy your application images and artifacts wherever you need, with reliability and speed.

![0003](/images/3/2/0003.svg?featherlight=false&width=4pc)

The application will utilize [AWS ECR private registry](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html) to securely store and manage container images used within the AWSDSC-XUT team.

**AWS ECS**

[AWS ECS](https://docs.aws.amazon.com/ecs/) is a fully managed container orchestration service that simplifies deploying, managing, and scaling containerized applications. AWS ECS seamlessly integrates with AWS ECR and Docker, freeing your team to concentrate on creating powerful applications while it handles the heavy lifting of infrastructure management.

![0004](/images/3/2/0004.svg?featherlight=false&width=4pc)

AWSome Books might need [AWS ECS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html) to run containers without managing servers or EC2 instances. It eliminates the need to provision, configure, or scale clusters, freeing you from choosing server types, scaling decisions, and optimizing cluster packing. Simply focus on your containers, and Fargate handles the rest.

**AWS Auto Scaling**

AWS offers a range of services to help you scale your application seamlessly. [AWS Auto Scaling](https://docs.aws.amazon.com/autoscaling/) ensures your application scales efficiently and is included at no extra cost beyond the standard fees for other AWS resources.

![0005](/images/3/2/0005.svg?featherlight=false&width=4pc)

AWS Auto Scaling Group helps you ensure that you have the correct number of AWS ECS Fargate tasks available to handle the load for your application.

**AWS Application Load Balancer**

The [AWS Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html) (ALB) is a fully managed load balancing service that automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses, within one or more Availability Zones. It is designed to handle advanced routing and supports HTTP/HTTPS traffic, WebSocket, and serverless functions. ALB offers features like path-based and host-based routing, SSL termination, Web Application Firewall (WAF) integration, and monitoring through Amazon CloudWatch, making it ideal for modern web applications and microservices architectures.

![0006](/images/3/2/0006.svg?featherlight=false&width=4pc)

The AWS ALB might route the incoming traffics to AWS ECS Fargate tasks within the target group.


**AWS API Gateway**

[AWS API Gateway](https://docs.aws.amazon.com/apigateway/) lets you easily create and deploy APIs at any scale. You can build secure and scalable APIs that connect with AWS services, other web services, or data in the AWS Cloud, and make them available for your own apps or third-party developers.

![0007](/images/3/2/0007.svg?featherlight=false&width=4pc)

AWS API Gateway provides multiple API options, including [REST APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html), [HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html), and [WebSocket APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api.html). While AWSome Books can utilize either REST APIs or HTTP APIs, it may benefit from the advanced features that come with the REST APIs type offered by AWS API Gateway.

{{% notice info %}}
Discover the key differences between REST APIs and HTTP APIs [here](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html). 
{{% /notice %}}

**AWS Network Load Balancer**

The [AWS Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html) (NLB) is a high-performance service designed to handle millions of requests per second with ultra-low latency. Operating at Layer 4 of the OSI model, it balances TCP, UDP, and TLS traffic across EC2 instances, containers, or IP addresses within multiple Availability Zones. NLB supports static IP addresses, preserves source IPs, and integrates seamlessly with AWS Global Accelerator, making it ideal for latency-sensitive applications and real-time communication.

![0008](/images/3/2/0008.svg?featherlight=false&width=4pc)

The AWSDSC-XUT team aims to make the AWS ALB internal and accessible only through the AWS API Gateway with REST APIs type. REST APIs type, however, lacks built-in support for direct integration with an internal ALB. To solve this, you set up a VPC Link in API Gateway and use an AWS NLB as a pass-through service to seamlessly proxy requests from the VPC Link to the internal ALB.

<!-- **AWS Certificate Manager**

[AWS Certificate Manager](https://docs.aws.amazon.com/acm/) (ACM) helps you to provision, manage, and renew publicly trusted TLS certificates on AWS based websites.

![0009](/images/3/2/0009.svg?featherlight=false&width=4pc)

AWS Certificate Manager integrates seamlessly with AWS API Gateway to secure user connections over the public Internet, ensuring robust encryption and enhanced security for your APIs.

**AWS Cognito**

[AWS Cognito](https://docs.aws.amazon.com/cognito/) simplifies user authentication and authorization for your web and mobile apps. With user pools, you can effortlessly add secure sign-up and sign-in capabilities, while identity pools (federated identities) provide temporary credentials for users to access specific AWS resources—whether they are signed in or browsing anonymously."

![00010](/images/3/2/00010.svg?featherlight=false&width=4pc) -->

**AWS CodeDeploy**

[AWS CodeDeploy](https://docs.aws.amazon.com/codedeploy/) is a deployment service that enables developers to automate the deployment of applications to instances and to update the applications as required.

![00011](/images/3/2/00011.svg?featherlight=false&width=4pc)

AWS CodeDeploy offers built-in support for Blue/Green deployments with ECS tasks, saving you the hassle of building a custom solution from scratch. Simplify your deployments and focus on scaling your applications with ease!

**AWS Chatbot**

[AWS Chatbot](https://docs.aws.amazon.com/chatbot/) allows you to monitor and respond to AWS operational events directly in Amazon Chime, Microsoft Teams, or Slack. It processes events from Amazon SNS, delivers notifications to chat channels, and supports AWS User Notifications. With AWS Chatbot, you can diagnose issues, run CLI commands, retrieve resource info, and identify remediation paths through a conversational interface.

![00012](/images/3/2/00012.svg?featherlight=false&width=4pc)

While AWS Chatbot offers great potential, the AWSDSC-XUT team might want to start by centralizing AWS CodeDeploy notifications to Slack, alongside GitHub Actions workflow notifications, for a more streamlined approach.

#### The Security of Connections

Without public subnets and Internet Gateway settings, the AWSDSC-XUT team intends to maintain the VPC as nearly private as possible. However, as previously indicated, you may still access your private resources by configuring an AWS API Gateway to enable communication over AWS's private network via a VPC Link. Moreover, your private resources can securely access public AWS services over AWS's private network by using the necessary VPC Endpoints.

{{% notice tip %}}
You can set up a [bastion host](https://aws.amazon.com/vi/solutions/implementations/linux-bastion/) in a public subnet to securely access your private resources when necessary.
{{% /notice %}}

You may be wondering about the security of connections from users to the AWS API Gateway and between AWS Services. Take a look at the figure below for a detailed view:

![00020](/images/3/2/00020.svg?featherlight=false&width=100pc)

*If you need complete control over the view of the image, check [here](https://drive.google.com/file/d/1lQf9hXIX_CY9qdxJ2nUAiqfiJJdn0G2b/view?usp=sharing).*

In summary:

- Connections from users to AWS API Gateway are encrypted over the Internet.
- Communications between AWS Services can be either encrypted or unencrypted over AWS's private network.

{{% notice tip %}}
For the highest level of security, it is recommended to encrypt all connections.
{{% /notice %}}

#### AWS Architecture

You might build the following AWS architecture for the AWSome Books application, including CI/CD pipelines and other technologies:

![AWS architecture diagram](/images/0/0001.svg?featherlight=false&width=100pc)

*If you need complete control over the view of the image, check [here](https://drive.google.com/file/d/1YuVMfHeR6bTuoOPd-djev-ztyRLQvyeq/view?usp=sharing).*

 You now should not be surprised by the architecture where all subnets are private and no Internet Gateway configurations. We have covered this in detail throughout this section already.