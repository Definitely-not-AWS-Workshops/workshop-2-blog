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

**5.** Click on the **Actions** tab, and you notice that the Update Dependency Cache workflow has been triggered.

![0005](/images/11/9/0005.svg?featherlight=false&width=100pc)

**6.** Once the workflow execution has completed, click **Caches** to navigate to the GitHub Actions Cache.

![0006](/images/11/9/0006.svg?featherlight=false&width=100pc)

**7.** Check the GitHub Actions Cache to view the cache for the main branch. Now, other branches should be able to restore their dependencies from this cache, as long as the specific key remains unchanged.

![0007](/images/11/9/0007.svg?featherlight=false&width=100pc)