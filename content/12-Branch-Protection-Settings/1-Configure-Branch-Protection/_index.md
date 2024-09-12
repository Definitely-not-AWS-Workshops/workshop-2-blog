---
title : "Configure Branch Protection"
date : "`r Sys.Date()`"
weight : 12
chapter : false
pre : " <b> 12.1 </b> "
---

You are going to set up minimal rules to experiment with branch protection on the *main* branch.

**1.** Under your remote repository name,

- Select **Settings** tab.
- Choose **Branches**.
- Click **Add classic branch protection rule**.

![0001](/images/12/1/0001.svg?featherlight=false&width=100pc)

**2.** For **Branch name pattern**, enter `main`.

![0002](/images/12/1/0002.svg?featherlight=false&width=100pc)

**3.** Enable **Require a pull request before merging**.

![0003](/images/12/1/0003.svg?featherlight=false&width=100pc)

**4.** Enable **Require status checks to pass before merging** and **Require branches to be up to date before merging**.

![0004](/images/12/1/0004.svg?featherlight=false&width=100pc)

**5.** Filter with `Run unit tests` value and select it.

![0005](/images/12/1/0005.svg?featherlight=false&width=100pc)

**6.** Repeat step **5**, but this time use the values `Run integration tests`, `Build image`, `Scan image`, and `Scan source code`. You will then have a total of 5 checks that must pass before merging back into the *main* branch.

![0006](/images/12/1/0006.svg?featherlight=false&width=100pc)

**7.** Enable **Do not allow bypassing the above settings**.

![0007](/images/12/1/0007.svg?featherlight=false&width=100pc)

**8.** Scroll down to the bottom, click **Create**.

![0008](/images/12/1/0008.svg?featherlight=false&width=100pc)