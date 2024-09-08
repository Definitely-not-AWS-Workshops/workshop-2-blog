---
title : "Your First CI Workflow Execution"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 11.8 </b> "
---

You first need to run the CI workflow to facilitate further settings.

**1.** Make sure you are still in the right project folder [11.2 Get The Prepared Source Code](11-set-up-awsome-books-repository/2-get-the-prepared-source-code).

```git
cd path/to/awsome-books
```

**2.** Create and switch to the *config* branch.

```git
git checkout -b config
```

**3.** Make a commit and push to the remote repository.

```git
git add . && git commit -m "first config" && git push --set-upstream origin config
```

**4.** Go to the AWSome Books remote repository. Click **Compare & pull request**.

![0001](/images/11/8/0001.svg?featherlight=false&width=100pc)

**5.** Click **Create pull request**.

![0002](/images/11/8/0002.svg?featherlight=false&width=100pc)

**6.** A CI workflow might be triggred to verify your code. You are able to track the progress right from the pull request console.

![0003](/images/11/8/0003.svg?featherlight=false&width=100pc)

**7.** Alternatively, go to the **Actions** tab and select the workflow that's currently running.

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

**11.** Click the **Cache dependencies** step. Since it is the very first time the CI workflow run, you might see *Cache not found for input keys*.

![0009](/images/11/8/0009.svg?featherlight=false&width=100pc)

**12.** After completing the primary steps, the "post" steps may run to handle tasks such as cleanup or cache writing.

For instance, if you click on the **Post Cache dependencies** step, you'll see that it saves the downloaded dependencies using a specific key.

![00010](/images/11/8/00010.svg?featherlight=false&width=100pc)

**13.** Click **CI** button to 

![00011](/images/11/8/00011.svg?featherlight=false&width=100pc)

**14.** Click **CI** button to 

![00012](/images/11/8/00012.svg?featherlight=false&width=100pc)

**15.** Click **CI** button to 

![00013](/images/11/8/00013.svg?featherlight=false&width=100pc)

