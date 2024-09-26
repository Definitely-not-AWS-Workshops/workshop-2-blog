---
title : "Further Improvements"
date : "`r Sys.Date()`"
weight : 20
chapter : false
pre : " <b> 20. </b> "
---

By setting up AWS API Gateway with the REST APIs type, you now have access to advanced features that offer greater capabilities compared to the HTTP APIs type. Here are some advanced REST APIs configurations you may want to explore:

- Your AWS API Gateway is accessible to anyone with internet access. To enhance security, consider integrating [AWS Cognito](https://docs.aws.amazon.com/cognito/) with a user pool as an authorizer.

- Currently, your AWS API Gateway is using the default endpoint. For enhanced security and branding, consider setting up a custom domain with SSL/TLS encryption through [AWS Route 53](https://docs.aws.amazon.com/route53/) and [AWS Certificate Manager](https://docs.aws.amazon.com/acm//).

Your AWS architecture would then look like this:

![0001](/images/20/0001.svg?featherlight=false&width=100pc)