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

**6.** Make sure you are still in the right project folder.

```git
cd path/to/experiment-3
```

**7.** Switch to *person-a* branch.

```git
git checkout person-a
```

**8.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-a
```

**9.** On your remote repository, click **Compare & pull request**.

![0006](/images/13/4/0004.svg?featherlight=false&width=100pc)

**10.** Click **Create pull request**.

![0007](/images/13/2/0002.svg?featherlight=false&width=100pc)

**11.** On your local repository, switch to *person-b* branch.

```git
git checkout person-b
```

**12.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-b
```

**13.** On your remote repository, click **Compare & pull request**.

![0008](/images/13/4/0005.svg?featherlight=false&width=100pc)

**14.** Click **Create pull request**.

![0009](/images/13/2/0009.svg?featherlight=false&width=100pc)

**15.** You should now have passed CI workflow executions of both pull requests. Do not click **Merge pull request** yet.

- Console for the person *a*'s pull request.

![00010](/images/13/3/0003.svg?featherlight=false&width=100pc)

- Console for the person *b*'s pull request.

![00011](/images/13/3/0004.svg?featherlight=false&width=100pc)

**16.** On the person *a*'s pull request console, click **Merge pull request**.

![00012](/images/13/3/0005.svg?featherlight=false&width=100pc)

**17.** Click **Confirm merge**.

![00013](/images/13/3/0006.svg?featherlight=false&width=100pc)

**18.** Click **Delete branch**.

![00014](/images/13/3/0007.svg?featherlight=false&width=100pc)

**19.** On the person *b*'s pull request console, you should now see the **This branch is out-of-date with the base branch** notification. Before you manually update the branch, click **Details** to review the logs from the previous CI check.

![00015](/images/13/4/0006.svg?featherlight=false&width=100pc)

**20.** Click the **Show main.py content** step dropdown, you might notice that

- The default value of *n* should remain **DEFAULT**.
- The return value of the function has changed.

![00016](/images/13/4/0007.svg?featherlight=false&width=100pc)
