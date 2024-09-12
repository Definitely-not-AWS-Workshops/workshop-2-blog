---
title : "Your Third CI Workflow Execution"
date : "`r Sys.Date()`"
weight : 12
chapter : false
pre : " <b> 12.2 </b> "
---


Make sure you are still in the right project folder.

```git
cd path/to/awsome-books
```

#### Prevent Merging into the Main Branch If the CI Workflow Fails

**1.** In your local repository, checkout to the *main* branch.

```git
git checkout main
```

**2.** Use the following command to pull the latest codebase from the remote *main* branch.

```git
git pull
```

**3.** Create and switch to another branch named **ruleset-experiment**.

```git
git checkout -b ruleset-experiment
```

**4.** Run the following command to modify *README.md* file. Remember that your **build-image** job still run **exit 1**.

```git
echo "ruleset experiment!" > README.md
```

**5.** Make a commit and push to the remote **ruleset-experiment** repository.

```git
git add . && git commit -m "ruleset experiment" && git push --set-upstream origin ruleset-experiment
```

**6.** Go to the AWSome Books remote repository. Click **Compare & pull request**.

![0001](/images/12/2/0001.svg?featherlight=false&width=100pc)

**7.** Click **Create pull request**.

![0002](/images/12/2/0002.svg?featherlight=false&width=100pc)

**8.** Wait for the jobs to complete. You may notice that the **Merge pull request** button is now disabled because not all jobs have passed. Your *main* branch is now secure from any unverified code.

![0003](/images/12/2/0003.svg?featherlight=false&width=100pc)

You next modifies the CI workflow to get all the jobs run successfully.

**9.** In **.github/workflows/ci.yml** file on your local machine, remove the **run: exit 1** step from the **buid-image** job.

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

**10.** Make a commit and then push to the remote **ruleset-experiment** branch.

```git
git add . && git commit -m "remove failed job" && git push
```

**11.** In your pull request details page, the CI workflow should rerun again.

![0004](/images/12/2/0004.svg?featherlight=false&width=100pc)

**12.** Wait for all the jobs have completed, you then should see the **Merge pull request** button enable again. Click it.

![0005](/images/12/2/0005.svg?featherlight=false&width=100pc)

**13.** Click **Delete Branch**.

![000103](/images/12/2/000103.svg?featherlight=false&width=100pc)

#### Block Direct Pushes from the Local Main Branch to the Remote Main Branch.

Imagine a team member forgets to switch to a different branch and, after completing their work, commits and pushes directly from the local main branch to the remote main branch. Boom! Your remote codebase is broken, and you have extra work to fix it.

Fortunately, with branch protection configured, your main branch is now safe from this scenario.

**1.** In your local repository, checkout to the *main* branch.

```git
git checkout main
```

**2.** Use the following command to pull the latest codebase from the remote *main* branch.

```git
git pull
```

**3.** Add the following content to *README.md* file.

```git
echo "This is AWSome Books!" > README.md
```

**4.** Make a commit and push to the remote *main* repository.

```git
git add . && git commit -m "modify README.md file" && git push
```

You should no longer be able to push directly to the remote *main* branch thanks to branch protection.

![0006](/images/12/2/0006.svg?featherlight=false&width=100pc)

#### Bypass Ruleset (Optional)

 You can bypass the rules by adding the right actors to the **Bypass list** section in your ruleset setting page. For example, if the list includes roles like **Organization admin** or **Repository admin**, your GitHub account will have the ability to bypass the existing rules since you have organization or repository administrative permissions by default.

**1.** Turn back to the ruleset settings. Click **Add bypass** dropdown and then select **Organization admin** role.

![000100](/images/12/000100.svg?featherlight=false&width=100pc)

**2.** In your local AWSome Books repository, make a small change to any file of your choice. Then, create a commit and push it to the remote *main* branch.

```git
git add . && git commit -m "a small change to x file" && git push
```

You are now allowed to push directly to the remote *main* branch without being affected by the **Require a pull request before merging** rule.

![000101](/images/12/000101.svg?featherlight=false&width=100pc)

For this workshop, you should leave the **Bypass list** empty to maintain an experimental setup.
