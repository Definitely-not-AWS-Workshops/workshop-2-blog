---
title : "Branching Strategies"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

A branching strategy is a crucial component that affects your pipeline design. At the very beginning of a pipeline design, it must be apparent how the team operates and which process they use. Depending on the plan, the pipeline flow varies.

The most popular branching strategies for effective code collaboration include *feature branching*, *gitflow*, and *trunk-based development*. 

#### Long-Lived Branching Strategies

As [feature branching](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow) and [gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) approaches introduce long-lived branches that stay for days or even longer, they prevent the fast feedback idea of DevOps practices. Merging back these long-lived branches with a huge amount of code changes to the main codebase can result in 

- a slowdown in the code review process due to the large volume of changes being introduced all at once. This can overwhelm reviewers and make it difficult to thoroughly assess the quality of each change.
- a higher chances of code conflicts as multiple developers work on separate features for an extended period. These conflicts can be complex and time-consuming to resolve, further delaying the integration of changes into the main codebase.
- a higher rate of CI failures as the size and complexity of the changes introduce new bugs and errors that may not be immediately apparent. This can disrupt the development workflow and require additional time and effort to diagnose and address issues before they can be merged into the main codebase.

The AWSDSC-XUT team has past experience with *gitflow* branching strategy and its limits from other projects. They, of course, want to apply an alternative branching strategy that overcomes these constraints.  

#### Trunk-Based Development

[trunk-based development](https://trunkbaseddevelopment.com/) is considered a companion for DevOps principles. One of the most interesting ideas of this branching strategy is that it requires regular commits to the main branch, even if a feature has not yet been fully developed. The complexity of this branching strategy is relatively low compared to others. *Trunked-based development* comes in both its original form and several variations. 

##### Original form

The main branch is the only branch included in the original form of *trunk-based development*. Changes are pushed straight to the *main* (*trunk*) branch, which then generates release candidates. However, pushing code changes from a local copy of the *trunk* to the remote *trunk* without a code review process and CI checks in another branch could be highly risky. This means that in the original form of *trunk-based development*, the main branch is not always in a production-ready state. Though frequently pushing code changes to the main codebase may speed up the development process, it may also increase the risk of production outages and cause stress for the team if each software release is unsafe. 

{{% notice info %}}
*trunk-based development*, centered around a single main branch, aligns seamlessly with [extreme programming](https://en.wikipedia.org/wiki/Extreme_programming) practices. In this approach, two developers often work together to maintain high code quality, ensuring that every commit to the branch is clean, reliable, and well-maintained.
{{% /notice %}}

##### Another variation

A different variation of *trunk-based development* involves creating short-lived feature branches that can be used to verify code changes before merging them back into the trunk. These branches can last up to a day (or a few days). This sort of branching strategy involves proposing commits to the main branch via pull requests (PRs), conducting CI and code review against those PRs, and then merging the branch and its changes into the main branch once CI passes and PR is approved. Then, the application can be released straight from the main branch or via another release branch created from the main.

The following figure illustrates the idea of *trunk-based development* with short-lived feature branches:

![0001](/images/2/2/0001.svg?featherlight=false&width=32pc)

As previously stated, the general goal of *trunk-based development* is to commit back to the main codebase as frequently as feasible, even if a feature has not yet been fully completed, which may align with the fast feedback of the DevOps culture. You break down your task into small chunks, each of which corresponds to a feature branch and can be completed in less than one day. That being said, it is hard! And it may feel easier not to do it. Despite minimizing difficulties when working with long-lived feature branches on other branching strategies, *trunk-based development* requires experienced teams to control incompleted parts of the codebase. 

Here are some helpful techniques for making small and frequent PRs:

- divide the work into separate tasks that should take a maximum of one day or a few hours. Consider writing brief, stand-alone PRs for them, each with associated documentation and tests.
- work-in-progress features can be hidden from user access by using [feature flags](https://martinfowler.com/articles/feature-toggles.html), or they can be prevented from being compiled into the codebase altogether by using *build flags*.
- if working from a single large feature branch is unavoidable, look for sections that can be committed back and take the time to split and combine the corresponding pull requests.

---

The AWSDSC-XUT team might apply *trunk-based development* with short-lived feature branches to speed up the delivery of AWSome Books. In this workshop, we will not cover *feature flags* or *build flags* — though they may be essential for handling incomplete features or specific business needs — as their implementation could demand considerable effort from your development team to manage the more complex codebase (due to the flags). Despite the challenges that call for experienced engineers, the team is eager to embrace this strategy, which enhances CI/CD practices and minimizes the headaches of dealing with long-lived branches.
