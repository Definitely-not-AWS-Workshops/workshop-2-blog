---
title : "Application Architecture"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

Since the AWSDSC-XUT team might be responsible for building more complex serverless RESTful API application architecture, you just need to build up the serverless components required to facilitate minimal book administration operations. To meet AWS recommendations for running the application securely through isolated environments, these components will subsequently be deployed to two different accounts. The AWS serverless services in an account might include the following:

- 1 AWS API Gateway serves as a "front door" for incoming requests.
- 4 AWS Lambda functions for CRUD operations on books.
- 1 AWS DynamoDB table for storing book information.
- Other resources for storing artifacts for AWS SAM or AWS Lambda.
- AWS IAM resources required for deploying and running the application in a specific account.

Without AWS SAM, manually provisioning and managing these resources for a production-ready serverless application might be a challenge and error-prone.

The minimal application architecture in AWS for the AWSome Books project:

![AWS architecture diagram](/images/0/0001.svg?featherlight=false&width=100pc)

The next step is to build the team's workflow and CI/CD pipelines.    
