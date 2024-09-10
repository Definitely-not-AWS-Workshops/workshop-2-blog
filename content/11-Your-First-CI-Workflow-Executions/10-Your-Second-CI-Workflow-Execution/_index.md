---
title : "Your Second CI Workflow Execution"
date : "`r Sys.Date()`"
weight : 10
chapter : false
pre : " <b> 11.10 </b> "
---

Now, try creating a failed CI workflow from a pull request and merging it into the *main* branch. You may notice that your CI workflow execution restores the cache from the *main* branch for its dependencies which might help speed up the CI jobs. Additionally, GitHub will allow you to merge a failed workflow execution into the main branch if no protection rules are in place.


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

**6.** Additionally, add the `--offline` option to the **Run unit tests** and **Run integration tests** steps in the **unit-test** and **integration-test** jobs.

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

**11.**  Wait for the workflow to complete, and you will see that it has failed because the **Build image** job encountered an error, and thus the **Scan image** job did not run. However, you might notice that the **Run unit tests**, **Run integration tests**, and **Scan source code** jobs have each reduced their execution time by about 10 seconds each compared to those at step **8** in [11.8 Your First CI Workflow Execution](11-your-first-ci-workflow-executions/8-your-first-ci-workflow-execution).

![0004](/images/11/10/0004.svg?featherlight=false&width=100pc)

Let's dive into several jobs to gain a deeper understanding of your CI workflow execution.

**12.** Let's check the **Build image** job first.

![0005](/images/11/10/0005.svg?featherlight=false&width=100pc)

**13.** You notice that the **Build image** job has failed due to the `exit 1` command specified in step **5**, which prevented it from executing the subsequent core steps.

![0006](/images/11/10/0006.svg?featherlight=false&width=100pc)

**14.** Return to your CI workflow diagram and click on the **Run unit tests** job to view the job in details.

![0007](/images/11/10/0007.svg?featherlight=false&width=100pc)

**15.** Click the **Cache dependencies** step dropdown. You might notice that **Cache restored successfully** and **Cache restored from key** with the key is the same as step **7** in [11.9 Update Dependency Cache](11-your-first-ci-workflow-executions/9-update-cache-dependency). However, the cache key of from **refs/pull/1/merge** pull request also the same. You can check the **last used** attribute to check which cache was used. The **last used** attribute of **main** should be newer than **refs/pull/1/merge**.

![0008](/images/11/10/0008.svg?featherlight=false&width=100pc)

**16.** Due to a cache hit, there is no cache saved.

![0009](/images/11/10/0009.svg?featherlight=false&width=100pc)