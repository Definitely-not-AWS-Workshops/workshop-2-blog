---
title : "Your First Release Workflow Execution"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 16.2 </b> "
---

You try to run a Release process with the wrong SemVer format specified. Watch what happens!

**1.** On your remote **awsome-books** repository, click **Create a new release**.

![0001](/images/16/2/0001.svg?featherlight=false&width=100pc)

**2.**

- Select the **Releases** tab.
- Expand the **Choose a tag** dropdown.
- Enter with value `v00.0.1`.
- Click **Create new tag: v00.0.1**.

![0002](/images/16/2/0002.svg?featherlight=false&width=100pc)

**3.** Scroll down to the bottom, click **Publish release**.

![0003](/images/16/2/0003.svg?featherlight=false&width=100pc)

**4.** A notification has been sent to the **gha-release** channel in your Slack Workspace, signaling that a Release workflow has been triggered.

![0004](/images/16/2/0004.svg?featherlight=false&width=100pc)

**5.** You ought to eventually get a message stating that your Release workflow has failed. Press **Release #1** to see more details.

![0005](/images/16/2/0005.svg?featherlight=false&width=100pc)

**6.** The Release workflow may show an overall failure, specifically due to the "Validate semantic version" job not passing.

![0006](/images/16/2/0006.svg?featherlight=false&width=100pc)

Next, let's release AWSome Books using the correct SemVer format!

