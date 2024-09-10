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

**5.** Head over to the **Actions** tab, and you'll see that the Update Dependency Cache workflow has been triggered. Click on the running workflow to view more details.

![0005](/images/11/9/0005.svg?featherlight=false&width=100pc)

**6.** You might notice that the initial Update Dependency Cache workflow completed in about 30 seconds. Click on the **Update dependency cache** job to explore the details.

![0006](/images/11/9/0006.svg?featherlight=false&width=100pc)

**7.** Click **Cache dependencies** step dropdown. You should see **Cache not found for input keys**.

![0007](/images/11/9/0007.svg?featherlight=false&width=100pc)

**8.**

![0008](/images/11/9/0008.svg?featherlight=false&width=100pc)

**9.**

![0009](/images/11/9/0009.svg?featherlight=false&width=100pc)

**10.**

![000100](/images/11/9/000100.svg?featherlight=false&width=100pc)

**11.** Check the GitHub Actions Cache to view the cache for the main branch. Now, other branches should be able to restore their dependencies from this cache, as long as the specific key remains unchanged.

![000101](/images/11/9/000101.svg?featherlight=false&width=100pc)