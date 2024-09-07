---
title : "AWS Architecture"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

While the AWSDSC-XUT team will focus on building advanced features, you are going to concentrate on optimizing the core infrastructure for seamless operations and scalability. With a minimal working version of the [AWSome Books application]() already in place, your goal is to recommend and deploy the essential AWS services needed to efficiently run a containerized RESTful API application at scale.

Before we dive into the AWSome Books AWS architecture, let's first examine the key AWS services that can effortlessly support a containerized RESTful API application.

#### AWS Services

**AWS RDS**

![0001](/images/3/2/0001.svg?featherlight=false&width=4pc)

[AWS RDS](https://docs.aws.amazon.com/rds/) simplifies the setup, operation, and scaling of relational databases in the cloud, offering cost-efficient, resizable capacity while handling common administrative tasks.

The AWSome Books project leverages PostgreSQL as its relational database, fully supported by Amazon RDS. By enabling [Multi-AZ instance deployments](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZSingleStandby.html), AWS RDS ensures high availability and automatic failover, offering robust support and resilience for your database with a standby instance in another availability zone.

**AWS Secrets Manager**

[AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/) securely encrypts, stores, and retrieves credentials for databases and services, eliminating the need to hardcode them in your applications. By calling Secrets Manager when needed, you can enhance security, automate secret rotation, and manage access, protecting your IT resources and data effectively.

![0002](/images/3/2/0002.svg?featherlight=false&width=4pc)


With AWS RDS's built-in support for AWS Secrets Manager, you can keep your AWSome Books codebase clean and secure — no more hardcoding database credentials!

**AWS ECR**

[AWS ECR](https://docs.aws.amazon.com/ecr/) is a fully managed container registry that offers high-performance image hosting, allowing you to seamlessly deploy your application images and artifacts wherever you need, with reliability and speed.

![0003](/images/3/2/0003.svg?featherlight=false&width=4pc)

The application will utilize [AWS ECR private registry](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html) to securely store and manage container images used within the AWSDSC-XUT team.

**AWS ECS**

[AWS ECS](https://docs.aws.amazon.com/ecs/) is a fully managed container orchestration service that simplifies deploying, managing, and scaling containerized applications. With built-in AWS best practices and seamless integration with tools like AWS ECR and Docker, ECS lets your team focus on building applications rather than managing infrastructure.

![0004](/images/3/2/0004.svg?featherlight=false&width=4pc)

AWSome Books might need [AWS ECS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html) to run containers without managing servers or EC2 instances. It eliminates the need to provision, configure, or scale clusters, freeing you from choosing server types, scaling decisions, and optimizing cluster packing. Simply focus on your containers, and Fargate handles the rest.

**AWS Auto Scaling**

AWS offers a range of services to help you scale your application seamlessly. [AWS Auto Scaling](https://docs.aws.amazon.com/autoscaling/) ensures your application scales efficiently and is included at no extra cost beyond the standard fees for other AWS resources.

![0005](/images/3/2/0005.svg?featherlight=false&width=4pc)

AWS Auto Scaling Group helps you ensure that you have the correct number of AWS ECS Fargate tasks available to handle the load for your application.

**AWS Application Load Balancer**

The [AWS Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html) (ALB) is a fully managed load balancing service that automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses, within one or more Availability Zones. It is designed to handle advanced routing and supports HTTP/HTTPS traffic, WebSocket, and serverless functions. ALB offers features like path-based and host-based routing, SSL termination, Web Application Firewall (WAF) integration, and monitoring through Amazon CloudWatch, making it ideal for modern web applications and microservices architectures.

![0006](/images/3/2/0006.svg?featherlight=false&width=4pc)

The AWS ALB might route the incoming traffics to the target group including AWS ECS Fargate tasks.


**AWS API Gateway**

[AWS API Gateway](https://docs.aws.amazon.com/apigateway/) lets you easily create and deploy APIs at any scale. You can build secure and scalable APIs that connect with AWS services, other web services, or data in the AWS Cloud, and make them available for your own apps or third-party developers.

![0007](/images/3/2/0007.svg?featherlight=false&width=4pc)

AWS API Gateway provides multiple API options, including [REST APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html), [HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html), and [WebSocket APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-websocket-api.html). While AWSome Books can utilize either REST APIs or HTTP APIs, it may benefit from the advanced features and capabilities that come with the REST APIs type offered by API Gateway.

{{% notice info %}}
Discover the key differences between REST APIs and HTTP APIs [here](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html). 
{{% /notice %}}

**AWS Network Load Balancer**

The [AWS Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html) (NLB) is a high-performance service designed to handle millions of requests per second with ultra-low latency. Operating at Layer 4 of the OSI model, it balances TCP, UDP, and TLS traffic across EC2 instances, containers, or IP addresses within multiple Availability Zones. NLB supports static IP addresses, preserves source IPs, and integrates seamlessly with AWS Global Accelerator, making it ideal for latency-sensitive applications and real-time communication.

![0008](/images/3/2/0008.svg?featherlight=false&width=4pc)

The AWSDSC-XUT team aims to make the AWS ALB internal and accessible only through the AWS API Gateway using REST APIs. However, REST APIs in API Gateway lack built-in support for direct integration with an internal ALB. To solve this, you set up a VPC Link in API Gateway and use an AWS NLB as a pass-through service to seamlessly proxy requests from the VPC Link to the internal ALB.

#### User-AWS Service Interactions

As mentioned above, the VPC is entirely private and does not have Internet access. You, however, can still reach your private resources by configuring an AWS API Gateway with a VPC Link, enabling connectivity through AWS’s private network. 

{{% notice tip %}}
You can set up a [bastion host](https://aws.amazon.com/vi/solutions/implementations/linux-bastion/) to securely access your private resources when necessary.
{{% /notice %}}

You may be wondering about the security of connections from users to the API Gateway and between AWS Services. Take a look at the figure below for a detailed view:

![00020](/images/3/2/00020.svg?featherlight=false&width=100pc)

*If you need complete control over the view of the image, check [here](https://drive.google.com/file/d/1N3jMhLBQQzXQKa8JfW5R7RGvMHI2_wnb/view?usp=sharing).*

In summary:

- connections from users to AWS API Gateway are encrypted over the Internet.
- communications between AWS Services can be either encrypted or unencrypted over AWS's private network.

{{% notice tip %}}
For the highest level of security, it is recommended to encrypt all connections.
{{% /notice %}}




#### AWS Architecture

You might build the following AWS architecture for the AWSome Books application, including CI/CD pipelines and other technologies:

![AWS architecture diagram](/images/0/0001.svg?featherlight=false&width=100pc)

*If you need complete control over the view of the image, check [here](https://drive.google.com/file/d/1N3jMhLBQQzXQKa8JfW5R7RGvMHI2_wnb/view?usp=sharing).*

 You should not be surprised by the architecture where all subnets are private and no Internet Gateway configurations. We have covered this in detail throughout this section already.