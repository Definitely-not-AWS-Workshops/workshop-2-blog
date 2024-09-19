---
title : "Experiment 4"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 13.6 </b> "
---


You should set up the required code repositories in [13.2 Prepare Source Code](13-experiments-with-gitHub-actions-merge-group/2-prepare-source-code) if you have not done it yet.

...

The merge queue provides the same benefits as the **Require branches to be up to date before merging** branch protection, but does not require a pull request author to update their pull request branch and wait for status checks to finish before trying to merge.

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

**18.** Click **Confirm merge when ready**. Your pull request now should be added to the merge queue.

![00013](/images/13/6/00010.svg?featherlight=false&width=100pc)

**19.** On the person *b*'s pull request console, click **Merge when ready**.

![00014](/images/13/6/00011.svg?featherlight=false&width=100pc)

**20.** Click **Confirm merge when ready**. Your pull request now should be added to the merge queue.

![00015](/images/13/6/00012.svg?featherlight=false&width=100pc)

**21.** Click **merge queue** to see the queue. 

![00016](/images/13/6/00013.svg?featherlight=false&width=100pc)

**22.** Your merge queue should now contain two pull requests, with pull request **#1** which is the person *a*'s pull request at the front of the line. 

![00017](/images/13/6/00014.svg?featherlight=false&width=100pc)

Wait for all pull request merges to be completed and fully dequeued, then move to the next step.

**23.**  On the person *a*'s pull request console, you might notice that this pull request has been merged into the *main* branch since all checks have passed. Click **Details** to discover the logs.

![00017](/images/13/6/00015.svg?featherlight=false&width=100pc)

**24.** You can see that the checkout codebase should include just the change of the person *a*. 

![00018](/images/13/6/00016.svg?featherlight=false&width=100pc)

**25.** In the queue, since person *a*'s code changes have passed all tests after being integrated with the *main* branch, his pull request should successfully merge into the *main* branch.

![00019](/images/13/6/00017.svg?featherlight=false&width=100pc)

**26.** On person *b*'s pull request console, you might see a notification **github-merge-queue bot removed this pull request from the merge queue due to failed status checks**. Click **Details** to explore further.

![00020](/images/13/6/00018.svg?featherlight=false&width=100pc)

**27.** As expected with how the merge queue operates, under the **Show main.py content** dropdown, you might notice that person *b*'s pull request includes both person *a*'s changes and the current state of the *main* branch. As a result, person *b*'s pull request is expected to fail and will be rejected from merging into the *main* branch.

![00021](/images/13/6/00019.svg?featherlight=false&width=100pc)

In the previous experiment, the **Require branches to be up to date before merging** rule would block other pull requests if one was ahead, requiring you to wait for the preceding pull requests to complete before manually merging yours into the *main* branch. In contrast, the merge queue streamlines this process by allowing multiple pull requests to enter the queue at once. It automatically integrates each pull request with those ahead of it and the latest *main* codebase. While there may still be cases where your pull request has not passed CI checks and requires correction, if it does pass, your code changes can be merged into the *main* branch automatically — eliminating the need to wait for preceding pull requests to be processed and merging your pull request manually.
