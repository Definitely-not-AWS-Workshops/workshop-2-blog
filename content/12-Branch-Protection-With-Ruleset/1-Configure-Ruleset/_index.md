---
title : "Configure Ruleset"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 12.1 </b> "
---

Start configuring the ruleset by getting familiarized with the basic rules that control how users interact with the *main* branch.

{{% notice tip %}}
Consider enabling advanced configurations as needed in real-world scenarios.
{{% /notice %}}

**1.** Under your remote repository name,

- Select **Settings** tab.
- Choose **Branches**.
- Click **Add branch ruleset**.

![0001](/images/12/1/0001.svg?featherlight=false&width=100pc)

**2.** 

- For **Ruleset Name**, enter `main-protection`.
- For **Enforcement status**, select `Active`.

![0002](/images/12/1/0002.svg?featherlight=false&width=100pc)

**3.** In the **Target branches** section,

- Click **Add target** dropdown.
- Choose **Include default branch**. Your default branch should be *main* branch.

![0003](/images/12/1/0003.svg?featherlight=false&width=100pc)

**4.** Enable **Restrict deletions**.

![0004](/images/12/1/0004.svg?featherlight=false&width=100pc)

**5.** Enable **Require a pull request before merging**.

![0005](/images/12/1/0005.svg?featherlight=false&width=100pc)

**6.** Enable **Require status checks to pass** and **Do not require status checks on creation**.

![0006](/images/12/1/0006.svg?featherlight=false&width=100pc)

**7.** Add the following check,

- Click the **Add checks** dropdown.
- Filter with `Run unit tests` value.
- Select **Run unit tests**.

![0007](/images/12/1/0007.svg?featherlight=false&width=100pc)

**8.** Repeate step **7**, replace **Run unit tests** with `Run integration tests`, `Scan image`, `Scan source code`, and `Build image`. You finally get a total of 5 checks required to pass.

![0008](/images/12/1/0008.svg?featherlight=false&width=100pc)

**9.** Enable **Block force pushes**.

![0009](/images/12/1/0009.svg?featherlight=false&width=100pc)

**10.** Scroll down to the bottom, click **Save changes**.

![00010](/images/12/1/00010.svg?featherlight=false&width=100pc)


