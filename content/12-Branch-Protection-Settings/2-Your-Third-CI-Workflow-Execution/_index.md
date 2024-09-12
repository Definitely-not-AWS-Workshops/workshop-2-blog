---
title : "Your Third CI Workflow Execution"
date : "`r Sys.Date()`"
weight : 12
chapter : false
pre : " <b> 12.2 </b> "
---

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

![0001](/images/11/10/0001.svg?featherlight=false&width=100pc)

**8.** Click **Create pull request**.

![0002](/images/11/10/0002.svg?featherlight=false&width=100pc)

**x.** You can bypass the rules by adding the right actors to the Bypass list section. For example, if the list includes roles like **Organization admin** or **Repository admin**, your GitHub account will have the ability to bypass the existing rules since you have organization or repository administrative permissions by default.

Turn back to the ruleset settings. Click **Add bypass** dropdown and then select **Organization admin** role.

![000100](/images/12/000100.svg?featherlight=false&width=100pc)

**x.** In your local AWSome Books repository, make a small change to any file of your choice. Then, create a commit and push it to the remote *main* branch.

```git
git add . && git commit -m "a small change to x file" && git push
```

You are now allowed to push directly to the remote *main* branch without being affected by the **Require a pull request before merging** rule.

![000101](/images/12/000101.svg?featherlight=false&width=100pc)

For this workshop, you should leave the **Bypass list** empty to maintain an experimental setup.
