---
title : "Experiment 3"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 13.4 </b> "
---

You should set up the required code repositories in [13.1 Prepare Source Code](13-experiments-with-gitHub-actions-merge-group/1-prepare-source-code) if you have not done it yet.

In this section, you are going to start by creating a pull request for person *a*'s changes, followed by a separate pull request for person *b*'s. Once both pull requests are up, you will wait for the CI workflows to run. Since person *a*'s changes have not been merged into the *main* branch yet, person *b*'s CI check completes successfully without incorporating person *a*'s updates. Feeling confident, you merge person *b*'s code changes into the *main* branch. But then — boom! The codebase is now broken, even though both pull requests had passed all their tests.

**1.** On the remote repository,

- Select **Settings** tab.
- Select **Branches** on the left sidebar.
- Click **Add branch ruleset**.

![0001](/images/13/4/0001.svg?featherlight=false&width=100pc)

**2.**

- For **Ruleset Name**, enter `main-protection`.
- For **Enforcement status**, select **Active**.

![0002](/images/13/4/0002.svg?featherlight=false&width=100pc)

**3.** In the **Target branches** section,

- Click **Add target** dropdown.
- Choose **Include default branch**. Your default branch should be *main* branch.

![0003](/images/12/1/0003.svg?featherlight=false&width=100pc)

**4.**

- Enable **Require status checks to pass**.
- Enable **Require branches to be up to date before merging**.
- Enable **Do not require status checks on creation**.
- Click the **Add checks** dropdown.
- Filter with `run-test` value.
- Select **Add run-test**.

![0004](/images/13/4/0003.svg?featherlight=false&width=100pc)

**5.** Scroll down to the bottom, click **Create**.

![0005](/images/12/1/00010.svg?featherlight=false&width=100pc)

**1.** Make sure you are still in the right project folder.

```git
cd path/to/experiment-3
```

**2.** Switch to *person-a* branch.

```git
git checkout person-a
```

**3.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-a
```

**4.** On your remote repository, click **Compare & pull request**.

![0001](/images/13/3/0001.svg?featherlight=false&width=100pc)

**5.** Click **Create pull request**.

![0002](/images/13/2/0002.svg?featherlight=false&width=100pc)

**6.** On your local repository, switch to *person-b* branch.

```git
git checkout person-b
```

**7.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-b
```

**8.** On your remote repository, click **Compare & pull request**.

![0003](/images/13/3/0002.svg?featherlight=false&width=100pc)

**9.** Click **Create pull request**.

![0004](/images/13/2/0009.svg?featherlight=false&width=100pc)

**10.** You should now have passed CI workflow executions of both pull requests. Do not click **Merge pull request** yet.

- Console for the person *a*'s pull request.

![0005](/images/13/3/0003.svg?featherlight=false&width=100pc)

- Console for the person *b*'s pull request.

![0006](/images/13/3/0004.svg?featherlight=false&width=100pc)
