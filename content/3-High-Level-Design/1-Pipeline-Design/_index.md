---
title : "Pipeline Design"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

<!-- In this section, you first understand what CI/CD refers to in the workshop. You then examine suitable branching and deployment strategies that help deliver the application reliably and faster. After that, you define the team workflow and CI/CD pipelines using the Business Process Model and Notation 2.0. Lastly, you provide the overall AWS architecture that has all the services needed for the development and delivery of AWSome Books, which are going to be implemented in the hands-on sections.  -->
 
#### GitHub Actions

![0001](/images/3/1/0001.png?featherlight=false&width=5pc)

[GitHub Actions](https://github.com/features/actions) is a robust CI/CD and automation tool embedded within GitHub, designed to streamline the development workflow. Here are its key features:

- *seamless integration:* directly integrated with GitHub repositories, enabling smooth automation of development processes.
- *custom workflows:* create and manage workflows using YAML configuration files to automate tasks such as building, testing, and deploying code.
- *extensive marketplace:* access a wide range of pre-built actions for various tasks, enhancing functionality and saving time.
- *cross-platform support:* run workflows across multiple operating systems and environments either GitHub-hosted or self-hosted, including Linux, Windows, and macOS.
- *complex workflow management:* utilize features like conditions and matrix builds to handle sophisticated automation scenarios efficiently.
  
These features collectively enhance productivity and streamline the CI/CD process, making GitHub Actions a valuable tool for developers.

#### Slack

![0002](/images/3/1/0002.png?featherlight=false&width=6pc)

Slack is a collaborative messaging platform designed to enhance team communication and productivity. It provides a centralized space for conversations, file sharing, and integration with various tools and services. Key selling points of Slack include:

- *organized communication:* channels help keep conversations organized by topic, project, or team, making it easy to find and reference past discussions.
- *real-time messaging:* facilitate instant communication with direct messages and group chats, improving team responsiveness and collaboration.
- *integration capabilities:* connect with a wide range of apps and services, such as Google Drive, GitHub, and Trello, to streamline workflows and keep everything in one place.
- *search functionality:* powerful search features allow users to quickly find messages, files, and conversations, enhancing information retrieval.
- *customization and automation:* customize notifications and use workflows to automate repetitive tasks, increasing efficiency and reducing manual work.

These features make Slack a versatile and effective tool for enhancing team collaboration and streamlining communication.

#### Business Process Model and Notation (BPMN) 2.0

[BPMN](https://www.omg.org/spec/BPMN#document-metadata) - has become the de-facto standard for business processes diagrams (you should be familiar with it if you are a Business Analyst). It is meant for usage directly by stakeholders who create, manage, and implement business processes. A pipeline design defines the orchestration of a process or workflow and has many analogies to modeling business processes. The business process modeling paradigm can thus serve as the basis for pipeline design. BPMN diagrams used to build workflows or CI/CD pipelines provide insight into software delivery processes, such as stages, activities, and relationships with other systems. 

The notation used in this workshop is BPMN 2.0. BPMN 2.0 has a unique notation with identifiable icons known as *elements*. The table below shows a subset of the most used BPMN 2.0 elements that might be enough for designing your pipelines.

![0003](/images/2/2/0003.svg?featherlight=false&width=40pc)

#### The Proposal Workflow and Pipelines

Since AWS CodePipeline does not support stage execution in parallel

![0003](/images/3/1/00069.svg?featherlight=false&width=100pc)

![0003](/images/3/1/00070.svg?featherlight=false&width=100pc)

![0003](/images/3/1/00071.svg?featherlight=false&width=100pc)

In the next section, you learn how to improve the pipelines

