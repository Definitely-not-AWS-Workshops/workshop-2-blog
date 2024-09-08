---
title : "Yet another CI/CD Definition!"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

One of the most often used buzzwords in DevOps space is CI/CD, which has different contradicting definitions. I might take the definitions from the book [continuous delivery](https://www.amazon.co.uk/Grokking-Continuous-Delivery-Christie-Wilson/dp/1617298255):

-  **Continuous Integration (CI)** is the process of combining code changes frequently, with each change verified on check-in.
-  **Continuous Delivery (CD)** involves being able to release code changes safely at any time, with the process being as simple as pushing a button.

{{% notice note %}}
**Continuous Deployment** is another CD that automatically triggers a deployment to production environment whenever commits are integrated into the *main* branch. While powerful, it requires a mature development process and team to implement effectively. In this workshop, "CD" refers to **Continuous Delivery**.
{{% /notice %}}

With these definitions, do you ensure that you perform CI/CD properly (at least in its most basic form)?

To reliably deploy code changes at any time (CD), your code changes (commits) must *at least* go through extensive testings and code reviews via the CI process in another branch before being merged into the *main* branch. This makes CI and CD tightly coupled; you cannot be ready for deployment until your code is fully checked and ready to function as expected. 

{{% notice info %}}
Even with a comprehensive CI process, there is always a chance something could go wrong during a software release. To handle these situations effectively, a smart deployment strategy is key. You explore this further in Section [2.4 Deployment Strategies](2-tool-agnostic-concepts/4-deployment-strategies).

{{% /notice %}}

**Take away**: ensure your *main* codebase (or *release* branch) as protected as possible.
