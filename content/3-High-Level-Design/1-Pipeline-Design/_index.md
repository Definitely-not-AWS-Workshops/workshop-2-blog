---
title : "Pipeline Design"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

<!-- In this section, you first understand what CI/CD refers to in the workshop. You then examine suitable branching and deployment strategies that help deliver the application reliably and faster. After that, you define the team workflow and CI/CD pipelines using the Business Process Model and Notation 2.0. Lastly, you provide the overall AWS architecture that has all the services needed for the development and delivery of AWSome Books, which are going to be implemented in the hands-on sections.  -->
 
#### Business Process Model and Notation (BPMN) 2.0

[BPMN](https://www.omg.org/spec/BPMN#document-metadata) - has become the de-facto standard for business processes diagrams (you should be familiar with it if you are a Business Analyst). It is meant for usage directly by stakeholders who create, manage, and implement business processes. A pipeline design defines the orchestration of a process or workflow and has many analogies to modeling business processes. The business process modeling paradigm can thus serve as the basis for pipeline design. BPMN diagrams used to build workflows or CI/CD pipelines provide insight into software delivery processes, such as stages, activities, and relationships with other systems. 

The notation used in this workshop is BPMN 2.0. BPMN 2.0 has a unique notation with identifiable icons known as *elements*. The table below shows a subset of the most used BPMN 2.0 elements that might be enough for designing your pipelines.

![0003](/images/2/2/0003.svg?featherlight=false&width=40pc)

#### The Proposal Workflow and Pipelines

Since AWS CodePipeline does not support stage execution in parallel


In the next section, you learn how to improve the pipelines

#### GitHub Actions

[GitHub Actions](https://github.com/features/actions)