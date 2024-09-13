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

**4.** In the **Branch rules** section,

- Enable **Restrict deletions**.
- Enable **Restrict updates**.
- Enable **Require merge queue**.

![0004](/images/12/1/0004.svg?featherlight=false&width=100pc)

![0005](/images/12/1/0005.svg?featherlight=false&width=100pc)

![0006](/images/12/1/0006.svg?featherlight=false&width=100pc)

![0007](/images/12/1/0007.svg?featherlight=false&width=100pc)

![0008](/images/12/1/0008.svg?featherlight=false&width=100pc)

![0009](/images/12/1/0009.svg?featherlight=false&width=100pc)

![00010](/images/12/1/00010.svg?featherlight=false&width=100pc)

![00011](/images/12/1/00011.svg?featherlight=false&width=100pc)

![00012](/images/12/1/00012.svg?featherlight=false&width=100pc)


