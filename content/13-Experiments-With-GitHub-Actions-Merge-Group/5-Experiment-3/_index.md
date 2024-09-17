---
title : "Experiment 3"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 13.5 </b> "
---

You should set up the required code repositories in [13.1 Prepare Source Code](13-experiments-with-gitHub-actions-merge-group/1-prepare-source-code) if you have not done it yet.

In this section, you are going to begin by securing your *main* branch with the *Require branches to be up to date before merging* rule. Next, you create a pull request for person *a*'s code changes, followed by a separate one for person *b*'s. After both pull requests are submitted, you wait for the CI workflows to complete. Since person *a*'s changes have not been merged into *main* branch yet, person *b*'s CI check passes successfully without including person *a*'s updates. Confidently, you merge person *a*'s pull request into the *main* branch. However, with the new branch protection rule in place, person *b*'s pull request triggers an "update required" notification immediately. At this point, you manually update the *person-b* branch, which then invokes a new CI workflow run that tests person *b*'s changes now integrated with person *a*'s updates in the *main* branch.

**1.** On the remote repository,

- Select **Settings** tab.
- Select **Branches** on the left sidebar.
- Click **Add branch ruleset**.

![0001](/images/13/5/0001.svg?featherlight=false&width=100pc)

**2.**

- For **Ruleset Name**, enter `main-protection`.
- For **Enforcement status**, select **Active**.

![0002](/images/13/5/0002.svg?featherlight=false&width=100pc)

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

![0004](/images/13/5/0003.svg?featherlight=false&width=100pc)

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

![0006](/images/13/5/0004.svg?featherlight=false&width=100pc)

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

![0008](/images/13/5/0005.svg?featherlight=false&width=100pc)

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

![00015](/images/13/5/0006.svg?featherlight=false&width=100pc)

**20.** Click the **Show main.py content** step dropdown, you might notice that the default value of *n* should be **DEFAULT** since the person *b* did not change it.

![00016](/images/13/5/0007.svg?featherlight=false&width=100pc)

**21.** Back to the person *b*'s pull request console, click **Update branch**.

![00017](/images/13/5/0008.svg?featherlight=false&width=100pc)

**22.** Another CI workflow execution should be triggered.

![00018](/images/13/5/0009.svg?featherlight=false&width=100pc)

Wait for seconds, you should see that the CI check has failed.

![00019](/images/13/5/00010.svg?featherlight=false&width=100pc)

**23.** Click **Details** to show the job's logs.

![00020](/images/13/5/00011.svg?featherlight=false&width=100pc)

**24.** Expand the **Shown main.py content** step dropdown, you might now notice that default value of *n* has turned into 7. This means that GitHub Actions Checkout now pulls in person *b*'s code changes, fully integrated with the latest updates from the *main* branch. As a result the CI check should be failed.

![00021](/images/13/5/00012.svg?featherlight=false&width=100pc)

In this experiment, you see that the **Require branches to be up to date before merging** rule forces the *person-b* branch to be synced with the *main* branch before merging. Though your *main* branch is now protected, this can lead to a scenario where one merge blocks others. For instance, imagine 100 pull requests, all with passing CI checks, ready to be merged into main. If one pull request merges first, the others will have to wait. Additionally, any pull request could fail later when rerun against the newly updated main *branch*. In the worst case, a single pull request might have to wait for 99 others to merge first before it can be integrated.

To handle with this issue at scale, you might want to leverage GitHub Actions Merge Group. Let's dive into the final experiment!