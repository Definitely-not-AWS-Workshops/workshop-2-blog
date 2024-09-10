---
title : "Configure Ruleset for Branch Protection"
date : "`r Sys.Date()`"
weight : 12
chapter : false
pre : " <b> 12. </b> "
---

Your *main* branch should be thoroughly protected against bad commits or destructive behaviors. In this workshop, you can start with basic rules in the ruleset to control how people can interact with branches and tags in a repository. 

{{% notice tip %}}
For more advanced configurations, consider enabling them as needed in real-world scenarios.
{{% /notice %}}


{{% notice info %}}
Learn more about [rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets) and [branch protection rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches).
{{% /notice %}}

**1.** Under your remote repository name,

- Select **Settings** tab.
- Choose **Branches**.
- Click **Add branch ruleset**.

![0001](/images/12/0001.svg?featherlight=false&width=100pc)


**2.** 

- For **Ruleset Name**, enter `main-protection`.
- For **Enforcement status**, select `Active`.

![0002](/images/12/0002.svg?featherlight=false&width=100pc)

**3.** In the **Target branches** section,

- Click **Add target** dropdown.
- Choose **Include default branch**. Your default branch should be *main* branch.

![0003](/images/12/0003.svg?featherlight=false&width=100pc)

**4.** In the **Branch rules** section,

- Enable **Restrict deletions**.
- Enable **Restrict updates**.
- Enable **Require merge queue**.

![0004](/images/12/0004.svg?featherlight=false&width=100pc)

- Enable **Restrict deletions**.
- Enable **Require a pull request before merging**.
- Enable **Block force pushes**.
- Scroll down to the bottom, click **Create**.


**x.** You can bypass the rules by adding the right actors to the Bypass list section. For example, if the list includes roles like **Organization admin** or **Repository admin**, your GitHub account will have the ability to bypass the existing rules since you have organization or repository administrative permissions by default.

Turn back to the ruleset settings. Click **Add bypass** dropdown and then select **Organization admin** role.

![000100](/images/12/000100.svg?featherlight=false&width=100pc)

**x.** In your local AWSome Books repository, make a small change to any file of your choice. Then, create a commit and push it to the remote *main* branch.

```git
git add . && git commit -m "a small change to x file" && git push
```

You are now allowed to push directly to the remote *main* branch without being affected by the **Require a pull request before merging** rule.

![000101](/images/12/000101.svg?featherlight=false&width=100pc)

For this workshop, you should leave the **Bypass list** empty to maintain an experimental setup.
