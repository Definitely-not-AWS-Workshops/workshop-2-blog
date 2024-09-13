---
title : "Other Experiments"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 12.3 </b> "
---

You now attempt a direct push to the GitHub repository and bypass the branch protection settings.

Make sure you are still in the right project folder.

```git
cd path/to/awsome-books
```

#### Push from the Local Main Branch to the Remote Main Branch Directly

A team member forgets to switch branches and ends up committing and pushing his unverified changes directly from the local *main* branch to the remote *main* branch. Just like that, your remote codebase is broken, leaving you with the headache of fixing the mess.

Fortunately, with branch protection configured, your *main* branch is now safe from this scenario.

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

You should no longer be able to push directly to the remote *main* branch thanks to branch protection. Make a pull request instead!

![0006](/images/12/3/0001.svg?featherlight=false&width=100pc)

#### Bypass Ruleset (Optional)

 You can bypass the rules by adding the right actors to the **Bypass list** section in your ruleset setting page. For example, if the list includes roles like **Organization admin** or **Repository admin**, your GitHub account will have the ability to bypass the existing rules since the account has organization or repository administrative permissions by default.

**1.** Turn back to the ruleset settings. In the **Bypass list** section,  click **Add bypass** dropdown and then select **Organization admin** role.

![0002](/images/12/3/0002.svg?featherlight=false&width=100pc)

**2.** In your local AWSome Books repository, make a small change to any file of your choice. Then, create a commit and push it to the remote *main* branch.

```git
git add . && git commit -m "a small change to x file" && git push
```

You are now allowed to push directly to the remote *main* branch without being affected by the **Require a pull request before merging** rule.

![0003](/images/12/3/0003.svg?featherlight=false&width=100pc)

For this workshop, you should leave the **Bypass list** empty to maintain an experimental setup.
