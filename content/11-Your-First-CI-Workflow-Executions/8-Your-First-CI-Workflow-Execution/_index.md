---
title : "Your First CI Workflow Execution"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 11.8 </b> "
---

Because some settings require your jobs to be executed to enable protection (GitHub does not automatically scan your *yaml* files to identify the necessary jobs for these settings), you may need to run the initial CI workflow to set up the required configurations.

**1.** Make sure you are still in the right project folder [11.2 Get The Prepared Source Code](11-your-first-ci-workflow-executions/2-get-the-prepared-source-code).

```git
cd path/to/awsome-books
```

**2.** Create and switch to the *config* branch.

```git
git checkout -b config
```

**3.** Make a commit and push to the remote **config** repository.

```git
git add . && git commit -m "first config" && git push --set-upstream origin config
```

**4.** Go to the AWSome Books remote repository. Click **Compare & pull request**.

![0001](/images/11/8/0001.svg?featherlight=false&width=100pc)

**5.** Click **Create pull request**.

![0002](/images/11/8/0002.svg?featherlight=false&width=100pc)

**6.** A CI workflow might be triggred to verify your code. You are able to track the progress right from the pull request console.

![0003](/images/11/8/0003.svg?featherlight=false&width=100pc)

**7.** Alternatively, go to the **Actions** tab and select the workflow that is currently running.

![0004](/images/11/8/0004.svg?featherlight=false&width=100pc)

**8.** You can see a diagram that visualizes the relationships between jobs in your GitHub Actions workflow. The figure below illustrates how the jobs **Build image**, **Run unit tests**, **Run integration tests**, and **Scan source code** run in parallel, while the **Scan image** job depends on the completion of the **Build image** job.

![0005](/images/11/8/0005.svg?featherlight=false&width=100pc)

Once all the jobs are complete, it will look like this.

![0006](/images/11/8/0006.svg?featherlight=false&width=100pc)

Each job have downloaded the necessary dependencies or libraries from the Internet rather than using the cache. Take note of the execution time for each job for comparison. You can later improve their execution time by implementing dependency caching.

**9.** Select **Run unit tests** job.

![0007](/images/11/8/0007.svg?featherlight=false&width=100pc)

**10.**  In addition to the steps you have defined in the *yaml* file, there are many other steps within the **Run unit tests** job. You can review the execution time and detailed logs for each step here, which will help you debug and enhance your pipeline.

![0008](/images/11/8/0008.svg?featherlight=false&width=100pc)

**11.** Click the **Cache dependencies** step. Since it is the very first time the CI workflow runs, you might see *Cache not found for input keys*.

![0009](/images/11/8/0009.svg?featherlight=false&width=100pc)

**12.** After completing the primary steps, the "post" steps may run to handle tasks such as cleanup or cache writing.

For instance, if you click on the **Post Cache dependencies** step, you might notice that it saves the downloaded dependencies using a specific key.

![00010](/images/11/8/00010.svg?featherlight=false&width=100pc)

**13.** Click **CI** button to navigate to list of CI workflow executions.

![00011](/images/11/8/00011.svg?featherlight=false&width=100pc)

**14.** Select **Caches**.

![00012](/images/11/8/00012.svg?featherlight=false&width=100pc)

**15.**  For this pull request (**refs/pull/1/merge**), the cache should be successfully saved. As outlined in Section [3.1 Pipeline Design](3-high-level-design/1-pipeline-design), any subsequent CI runs related to this pull request will leverage the saved cache for their jobs. This cache, however, is exclusive to this pull request and will not be accessible by other branches.

![00013](/images/11/8/00013.svg?featherlight=false&width=100pc)

With GitHub Actions, you have built your first pipeline and made it run smoothly with just a few simple steps on GitHub!









