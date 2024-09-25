---
title : "Your First Rollback Workflow Execution"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 18.1 </b> "
---

You now try to roll back an inexisting version and your Rollback workflow should allow that.

**1.** On your remote **awsome-books** repository,

- Select the **Actions** tab.
- Choose the **Rollback** workflow.
- Expand the **Run workflow** dropdown.
- Enter the value `v0.0.7`.
- Click **Run workflow**.

![0001](/images/18/1/0001.svg?featherlight=false&width=100pc)

**2.** A Rollback workflow should be triggered, click it to see more details.

![0002](/images/18/1/0002.svg?featherlight=false&width=100pc)

**3.** Wait for the workflow completes, you should notice that the workflow should fail since the version you want to roll back does not exist.

![0003](/images/18/1/0003.svg?featherlight=false&width=100pc)

**4.** In your Slack Workspace, you also receive roll back workflow fail to execute on **gha-rollback** channel. 

![0004](/images/18/1/0004.svg?featherlight=false&width=100pc)