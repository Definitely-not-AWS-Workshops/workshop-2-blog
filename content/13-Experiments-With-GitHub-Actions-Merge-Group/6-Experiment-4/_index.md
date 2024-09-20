---
title : "Experiment 4"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 13.6 </b> "
---


You should set up the required code repositories in [13.2 Prepare Source Code](13-experiments-with-gitHub-actions-merge-group/2-prepare-source-code) if you have not done it yet.


A [merge queue](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-a-merge-queue) enhances development speed by automating pull request merges into a busy branch and preventing issues caused by incompatible changes.

It offers the same benefits as the **Require branches to be up to date before merging** branch protection but eliminates the need for pull request authors to manually update their branches and wait for status checks before merging.

This feature is especially valuable for branches receiving a high volume of pull requests daily from multiple contributors.

Once a pull request passes all necessary branch protection checks, a user with write access can add it to the queue. The *merge queue* then ensures that the pull request's changes pass all required status checks against the latest version of the target branch and any other pull requests already in the queue.

**1.** On the remote repository,

- Select **Settings** tab.
- Select **Branches** on the left sidebar.
- Click **Add branch ruleset**.

![0001](/images/13/6/0001.svg?featherlight=false&width=100pc)

**2.** On the ruleset settings page,

- For **Ruleset Name**, enter `main-protection`.
- For **Enforcement status**, select **Active**.

![0002](/images/13/6/0002.svg?featherlight=false&width=100pc)

**3.** In the **Target branches** section,

- Click **Add target** dropdown.
- Choose **Include default branch**. Your default branch should be *main* branch.

![0003](/images/12/1/0003.svg?featherlight=false&width=100pc)

**4.** Enable **Require merge queue**.

![0004](/images/13/6/0003.svg?featherlight=false&width=100pc)

**5.**

- Enable **Require status checks to pass**.
- Click the **Add checks** dropdown.
- Filter with `run-test` value.
- Select **Add run-test**.

![0004](/images/13/6/0004.svg?featherlight=false&width=100pc)

**6.** Scroll down to the bottom, click **Create**.

![0005](/images/12/1/00010.svg?featherlight=false&width=100pc)

**7.** Make sure you are still in the right project folder.

```git
cd path/to/experiment-4
```

**8.** Switch to *person-a* branch.

```git
git checkout person-a
```

**9.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-a
```

**10.** On your remote repository, click **Compare & pull request**.

![0006](/images/13/6/0005.svg?featherlight=false&width=100pc)

**11.** Click **Create pull request**.

![0007](/images/13/3/0002.svg?featherlight=false&width=100pc)

**12.** On your local repository, switch to *person-b* branch.

```git
git checkout person-b
```

**13.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-b
```

**14.** On your remote repository, click **Compare & pull request**.

![0008](/images/13/6/0006.svg?featherlight=false&width=100pc)

**15.** Click **Create pull request**.

![0009](/images/13/3/0009.svg?featherlight=false&width=100pc)

**16.** You should now have passed CI workflow executions of both pull requests. Do not click **Merge when ready** yet.

- Console for the person *a*'s pull request.

![00010](/images/13/6/0007.svg?featherlight=false&width=100pc)

- Console for the person *b*'s pull request.

![00011](/images/13/6/0008.svg?featherlight=false&width=100pc)

**17.** On the person *a*'s pull request console, click **Merge when ready**.

![00012](/images/13/6/0009.svg?featherlight=false&width=100pc)

**18.** Click **Confirm merge when ready**. 

![00013](/images/13/6/00010.svg?featherlight=false&width=100pc)

 **19.** The person *a*'s pull request now should be added to the *merge queue*. Click **merge queue** to view the queue.

![00014](/images/13/6/00011.svg?featherlight=false&width=100pc)

**20.** You might notice that the person *a*'s pull request is now in the queue.

![00015](/images/13/6/00012.svg?featherlight=false&width=100pc)

**21.** On the person *b*'s pull request console, click **Merge when ready**.

![00016](/images/13/6/00013.svg?featherlight=false&width=100pc)

**22.** Click **Confirm merge when ready**.

![00017](/images/13/6/00014.svg?featherlight=false&width=100pc)

**23.** The person *b*'s pull request now should be added to the *merge queue*. Click **merge queue** to view the queue.

![00018](/images/13/6/00015.svg?featherlight=false&width=100pc)

**24.** Your merge queue should now contain two pull requests, with pull request **#1** is the person *a*'s pull request at the front of the line. 

![00019](/images/13/6/00016.svg?featherlight=false&width=100pc)

Wait for all pull request merges to complete and be fully dequeued, then move to the next step.

**25.**  On the person *a*'s pull request console, you might notice that this pull request has been merged into the *main* branch since all checks have passed. Click **Details** to discover the logs.

![00020](/images/13/6/00017.svg?featherlight=false&width=100pc)

**26.** You can see that the checkout codebase for the person *a*'s pull request should include just the change of the person *a*. 

![00021](/images/13/6/00018.svg?featherlight=false&width=100pc)

**27.** In the queue, since person *a*'s code changes have passed all tests after being integrated with the *main* branch, his pull request should successfully merge into the *main* branch.

![00022](/images/13/6/00019.svg?featherlight=false&width=100pc)

**28.** On person *b*'s pull request console, you might see the **github-merge-queue bot removed this pull request from the merge queue due to failed status checks** notification. Click **Details** to explore further.

![00023](/images/13/6/00020.svg?featherlight=false&width=100pc)

**29.** As expected with how the *merge queue* operates, under the **Show main.py content** dropdown, you might notice that person *b*'s pull request includes both person *a*'s changes and the current state of the *main* branch in additon to person *b*'s code changes. As a result, person *b*'s pull request is expected to fail and will be rejected from merging into the *main* branch.

![00024](/images/13/6/00021.svg?featherlight=false&width=100pc)

In the previous experiment, the **Require branches to be up to date before merging** rule would block other pull requests if one was ahead, requiring you to wait for the preceding pull requests to complete before manually merging yours into the *main* branch. In contrast, the *merge queue* streamlines this process by allowing multiple pull requests to enter the queue at once. It automatically integrates each pull request with those ahead of it and the latest *main* codebase. While there may still be cases where your pull request has not passed CI checks and requires correction, if it does pass, your code changes can be merged into the *main* branch automatically — eliminating the need to wait for preceding pull requests to be processed and merging your pull request manually.

While GitHub lets you activate both the **Require branches to be up to date before merging** rule and the merge queue, using both can be redundant. The *merge queue* effectively manages all pull requests simultaneously, automatically integrating each one with changes from preceding requests. This removes the need to wait for earlier pull requests to complete, making the merging process smoother and more efficient. In other words, once your code review is approved, you simply add your pull request to the queue and let it handle the rest. If your pull request fails, you can correct it without any further delays.

To boost commit rates and speed up merging code changes back into the *main* branch, the AWSome Books project might want to try a *merge queue*. Next, you simply activate the *merge queue*, and let it streamline and accelerate your merging process!
