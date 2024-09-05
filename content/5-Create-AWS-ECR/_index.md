---
title : "Create AWS ECR"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

You now create a AWS ECR private repository and push a dummy Docker image to it. 

**1.** Go to [AWS ECR console](https://console.aws.amazon.com/ecr/).

**2.** Click **Create**.

![0001](/images/4/0001.svg?featherlight=false&width=100pc)

**3.** In the **General settings** section,
- For **Visibility settings**, choose **Private**.
- For **Repository name**, enter `awsome-books`.
- For **Tag immutability**, enable it.

![0002](/images/4/0002.svg?featherlight=false&width=100pc)

Scroll down to the bottom. Click **Create repository**.

![0003](/images/4/0003.svg?featherlight=false&width=100pc)

**4.** Turn on [Docker Desktop](https://www.docker.com/products/docker-desktop/) or any program that can build and push your Docker image to AWS ECR repository. This workshop will use Docker Desktop as an example.

![0004](/images/4/0004.svg?featherlight=false&width=100pc)

**5.** Clone the following repository [github.com/Definitely-not-AWS-Workshops/ip-printer](https://github.com/Definitely-not-AWS-Workshops/ip-printer).

```git
git clone https://github.com/Definitely-not-AWS-Workshops/ip-printer
```

Change the directory to the repository you have just cloned.


```git
cd ip-printer
```

**6.** Run the following commands to build and push Docker image to AWS ECR repository,

- Retrieve an authentication token and authenticate your Docker client to your registry. Replace **\<YOUR-AWS-ACCOUNT-ID\>** with yours. Use the AWS CLI,

```git
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <YOUR-AWS-ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com
```

- Build your Docker image.

```git
docker build -t awsome-books .
```

- After the build completes, tag your image so you can push the image to this repository. Replace **\<YOUR-AWS-ACCOUNT-ID\>** with yours.

```git
docker tag awsome-books:latest <YOUR-AWS-ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/awsome-books:v0.0.0
```

- Push the image to your AWS ECR repository. Replace **\<YOUR-AWS-ACCOUNT-ID\>** with yours.

```git
docker push <YOUR-AWS-ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/awsome-books:v0.0.0
```

**7.** Check out your AWS ECR private repository to find the Docker image with the tag v0.0.0. Make a note of the **Image URI** using the **Copy URI** button for a later use.

![0005](/images/4/0005.svg?featherlight=false&width=100pc)
