---
title : "Your Second CI Workflow Execution"
date : "`r Sys.Date()`"
weight : 10
chapter : false
pre : " <b> 11.10 </b> "
---

Now, try creating a failed CI workflow execution from a pull request and merging it into the *main* branch. You may notice that your CI workflow execution restores the cache from the *main* branch for its dependencies which might help speed up the CI jobs. Additionally, GitHub will allow you to merge a failed workflow execution into the main branch if no protection rules are in place.


**1.** Make sure you are still in the right project folder [11.2 Get The Prepared Source Code](11-create-your-first-ci-workflow-executions/2-get-the-prepared-source-code).

```git
cd path/to/awsome-books
```

**2.** In your local repository, checkout to the *main* branch.

```git
git checkout main
```

**3.** Your local *main* branch should now contain only the *README.md* file. Use the following command to pull the latest codebase from the remote *main* branch.

```git
git pull
```

**4.** Create and switch to another branch named **failed-job**.

```git
git checkout -b failed-job
```

**5.** Now, simulate a failed job in your CI workflow. In your **.github/workflows/ci.yaml** file, add a new step to the **build-image** job that could cause the workflow to fail.

change from

```git
  build-image:
    name: Build image
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
```

to

```git
  build-image:
    name: Build image
    runs-on: ubuntu-latest
    steps:
      - run: exit 1

      - name: Checkout
        uses: actions/checkout@v4
```

**6.** Additionally, add the `--offline` option to the **Run unit tests** and **Run integration tests** steps in the **unit-test** and **integration-test** jobs, respectively.

- For step **Run unit tests** in **unit-test** job.

change from

```git
- name: Run unit tests
  run: |
    chmod +x gradlew
    ./gradlew test
```

to

```git
- name: Run unit tests
  run: |
    chmod +x gradlew
    ./gradlew test --offline
```

- For step **Run integration tests** in **integration-test** job.

change from

```git
- name: Run integration tests
  run: |
    chmod +x gradlew
    ./gradlew integrationTest
```

to

```git
- name: Run unit tests
  run: |
    chmod +x gradlew
    ./gradlew integrationTest --offline
```

**7.** Make a commit and then push to the remote **failed-job** branch.

```git
git add . && git commit -m "add a failed job" && git push --set-upstream origin failed-job
```

**8.** Go to the AWSome Books remote repository. Click **Compare & pull request**.

![0001](/images/11/10/0001.svg?featherlight=false&width=100pc)

**9.** Click **Create pull request**.

![0002](/images/11/10/0002.svg?featherlight=false&width=100pc)

**10.** Click the **Actions** tab and select the workflow that is currently running.

![0003](/images/11/10/0003.svg?featherlight=false&width=100pc)

**11.**  Wait for the workflow to complete, and you will see that it has failed because the **Build image** job encountered an error, and thus the **Scan image** job did not run. However, you might notice that the **Run unit tests**, **Run integration tests**, and **Scan source code** jobs have each reduced their execution time by about 10 seconds compared to those at step **8** in [11.8 Your First CI Workflow Execution](11-your-first-ci-workflow-executions/8-your-first-ci-workflow-execution).

![0004](/images/11/10/0004.svg?featherlight=false&width=100pc)

Let's dive into several jobs to gain a deeper understanding of your CI workflow execution.

**12.** Let's check the **Build image** job first.

![0005](/images/11/10/0005.svg?featherlight=false&width=100pc)

**13.** You notice that the **Build image** job has failed due to the `exit 1` command specified in step **5**, which prevented it from executing the subsequent core steps.

![0006](/images/11/10/0006.svg?featherlight=false&width=100pc)

**14.** Return to your CI workflow diagram and click on the **Run unit tests** job to view the job in details.

![0007](/images/11/10/0007.svg?featherlight=false&width=100pc)

**15.** Click the **Cache dependencies** step dropdown. You might notice that **Cache restored successfully** and **Cache restored from key** with the key is the same as step **7** in [11.9 Update Dependency Cache](11-your-first-ci-workflow-executions/9-update-dependency-cache). 

![0008](/images/11/10/0008.svg?featherlight=false&width=100pc)

The cache key for the **refs/pull/1/merge** pull request is also identical. To determine which cache was utilized, check the **last used** attribute. The **last used** attribute for **main** should be more recent than that of **refs/pull/1/merge**.

![0009](/images/11/10/0009.svg?featherlight=false&width=100pc)

**16.** Expand the **Post Cache dependencies** step dropdown, and you will see the messages **Cache hit occurred on the primary key** and **not saving cache**. This means a cache hit was detected, so no new cache is saved. You can verify this by checking the second figure in step **15**, where you will see no new cache entries saved.

![00010](/images/11/10/00010.svg?featherlight=false&width=100pc)

**17.** Under your remote repository,

- Select **Pull requests** tab.
- Choose your opened request **Failed job**.

![00011](/images/11/10/00011.svg?featherlight=false&width=100pc)

**18.** Click **Merge pull request**.

![00012](/images/11/10/00012.svg?featherlight=false&width=100pc)

**19.** Click **Confirm merge**.

![00013](/images/11/10/00013.svg?featherlight=false&width=100pc)

**20.** Click **Delete branch**.

![00014](/images/11/10/00014.svg?featherlight=false&width=100pc)

**21.** Boom!!! A problematic commit has just made its way into your *main* codebase. While this is just a simulation of a failed job, in real-world situations, GitHub without proper branch protection settings could let these bad code changes slip into your *main* codebase.

You can check the **.github/workflows/ci.yml** file in the remote *main* branch to see the step running **exit 1** in the **build-image** job.

![00015](/images/11/10/00015.svg?featherlight=false&width=100pc)

**22.** Click on the **Actions** tab, and you notice that the Update Dependency Cache workflow has been triggered. Click the running workflow to see more in details.

![00016](/images/11/10/00016.svg?featherlight=false&width=100pc)

**23.** Wait unitl the pipeline execution has completed.

![00017](/images/11/10/00017.svg?featherlight=false&width=100pc)

**24.**

![00018](/images/11/10/00018.svg?featherlight=false&width=100pc)

**25.**

![00019](/images/11/10/00019.svg?featherlight=false&width=100pc)

**26.**

![00020](/images/11/10/00020.svg?featherlight=false&width=100pc)