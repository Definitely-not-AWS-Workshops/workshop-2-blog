---
title : "Your Third CI Workflow Execution"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 12.2 </b> "
---

After completing your third execution of the CI workflow, you discover that GitHub required all of the checks to pass in order for the pull request to be merged into the main branch.

**1.** Make sure you are still in the right project folder.

```git
cd path/to/awsome-books
```

**2.** In your local repository, checkout to the *main* branch.

```git
git checkout main
```

**3.** Use the following command to pull the latest codebase from the remote *main* branch.

```git
git pull
```

**4.** Create and switch to another branch named **ruleset-experiment**.

```git
git checkout -b ruleset-experiment
```

**5.** Run the following command to modify *README.md* file. Remember that your **build-image** job still run **exit 1**.

```git
echo "ruleset experiment!" > README.md
```

**6.** Make a commit and push to the remote **ruleset-experiment** repository.

```git
git add . && git commit -m "ruleset experiment" && git push --set-upstream origin ruleset-experiment
```

**7.** Go to the AWSome Books remote repository. Click **Compare & pull request**.

![0001](/images/12/2/0001.svg?featherlight=false&width=100pc)

**8.** Click **Create pull request**.

![0002](/images/12/2/0002.svg?featherlight=false&width=100pc)

**9.** Wait for the jobs to complete. You may notice that the **Merge pull request** button is now disabled because not all jobs have passed. Your *main* branch is now secure from any unverified code.

![0003](/images/12/2/0003.svg?featherlight=false&width=100pc)

You next modifies the CI workflow to get all the jobs run successfully.

**10.** In **.github/workflows/ci.yml** file on your local machine, remove the **run: exit 1** step from the **buid-image** job.

change from

```yml
  build-image:
    name: Build image
    runs-on: ubuntu-latest
    steps:
      - run: exit 1

      - name: Checkout
        uses: actions/checkout@v4
```

to

```yml
  build-image:
    name: Build image
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
```

**11.** Make a commit and then push to the remote **ruleset-experiment** branch.

```git
git add . && git commit -m "remove failed job" && git push
```

**12.** In your pull request details page, the CI workflow should rerun again.

![0004](/images/12/2/0004.svg?featherlight=false&width=100pc)

**13.** Wait for all the jobs have completed, you then should see the **Merge pull request** button enable again. Click it.

![0005](/images/12/2/0005.svg?featherlight=false&width=100pc)

**14.** Click **Delete Branch**.

![0006](/images/12/2/0006.svg?featherlight=false&width=100pc)

