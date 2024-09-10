---
title : "Update Dependency Cache"
date : "`r Sys.Date()`"
weight : 9
chapter : false
pre : " <b> 11.9 </b> "
---

Your first CI run executed without restoring the cache from GitHub Actions Cache. Next, merge your pull request back into the *main* branch to automatically update the dependency cache.

**1.** Under your remote repository,

- Select **Pull requests** tab.
- Choose your opened request **first config**.

![0001](/images/11/9/0001.svg?featherlight=false&width=100pc)

**2.** Click **Merge pull request**.

![0002](/images/11/9/0002.svg?featherlight=false&width=100pc)

**3.** Click **Confirm merge**.

![0003](/images/11/9/0003.svg?featherlight=false&width=100pc)

**4.** Click **Delete branch**.

![0004](/images/11/9/0004.svg?featherlight=false&width=100pc)

**5.** Head over to the **Actions** tab, and you will see that the **Update dependency cache** workflow has been triggered. Click on the running workflow to view more details.

![0005](/images/11/9/0005.svg?featherlight=false&width=100pc)

**6.** You might notice that the **Update dependency cache** job completed in about 30 seconds. Click on the **Update dependency cache** job to explore the details.

![0006](/images/11/9/0006.svg?featherlight=false&width=100pc)

**7.** Expand the **Cache dependencies** step dropdown. You will see the message **Cache not found for input keys** since this is the first run of your **Update dependency cache** workflow, and no cache is restored from **refs/pull/1/merge**.

![0007](/images/11/9/0007.svg?featherlight=false&width=100pc)

**8.** Open the **Update cache** step dropdown. Here, you will see that this step is responsible for downloading all the required dependencies for the application since there is no cache hit in the previous step.

![0008](/images/11/9/0008.svg?featherlight=false&width=100pc)

**9.** Click **Post Cache dependencies** step dropdown. This step should perform the cache saving as it specifies **Cache saved successfully** and **Cache saved with key**.

![0009](/images/11/9/0009.svg?featherlight=false&width=100pc)

<!-- **10.**

![000100](/images/11/9/000100.svg?featherlight=false&width=100pc) -->

**10.** Check the GitHub Actions Cache to view the cache for the *main* branch. Now, other branches should be able to restore their dependencies from this cache, as long as the specific key remains unchanged.

![00010](/images/11/9/00010.svg?featherlight=false&width=100pc)