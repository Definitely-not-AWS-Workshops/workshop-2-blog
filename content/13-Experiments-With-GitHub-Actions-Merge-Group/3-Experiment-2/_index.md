---
title : "Experiment 2"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 13.3 </b> "
---

You should set up the required repositories in [13.1 Prepare Source Code](13-experiments-with-gitHub-actions-merge-group/1-prepare-source-code) if you have not done it yet.

In this section, you are going to start by creating a pull request for person *a*'s changes, followed by a separate pull request for person *b*'s. Once both pull requests are up, you will wait for the CI workflows to run. Since person *a*'s changes have not been merged into the *main* branch yet, person *b*'s CI check completes successfully without incorporating person *b*'s updates. Feeling confident, you merge person *b*'s code changes into the *main* branch. But then—boom! The codebase is now broken, even though both pull requests had passed all their tests.

Even when CI workflows pass on individual pull requests, your *main* codebase can still break if it is not tested with the latest merged code on the local machine. To help prevent this, let’s take the next step and create a basic branch protection rule to safeguard your workflow and reduce the chances of errors slipping through.