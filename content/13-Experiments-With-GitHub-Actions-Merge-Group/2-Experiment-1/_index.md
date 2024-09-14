---
title : "Experiment 1"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 13.2 </b> "
---

You should set up the required repositories in [13.1 Prepare Source Code](13-experiments-with-gitHub-actions-merge-group/1-prepare-source-code) if you have not done it yet. 

In this section, you are going to first merge person *a*'s code changes into the *main* branch. Once that’s done, you create a pull request to integrate person *b*'s updates. This action trigger the CI workflow, and you might discover something interesting along the way. Let's and explore!

**1.** Make sure you are still in the right project folder.

```git
cd path/to/experiment-1
```

**2.** Switch to *person-a* branch.

```git
git checkout person-a
```

**3.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-a
```

**4.** On your remote repository, click **Compare & pull request**.

![0001](/images/13/2/0001.svg?featherlight=false&width=100pc)

**5.** Click **Create pull request**.

![0002](/images/13/2/0002.svg?featherlight=false&width=100pc)

**6.** Wait for the job has passed and then click **Merge pull request**.

![0003](/images/13/2/0003.svg?featherlight=false&width=100pc)

**7.** Click **Confirm merge**.

![0004](/images/13/2/0004.svg?featherlight=false&width=100pc)

**8.** Click **Delete branch**.

![0005](/images/13/2/0005.svg?featherlight=false&width=100pc)

**9.** Click the **Code** tab and then open the *main.py* file.

![0006](/images/13/2/0006.svg?featherlight=false&width=100pc)

**10.** You might notice that the default value of *n* is now 7.

![0007](/images/13/2/0007.svg?featherlight=false&width=100pc)

**11.** On your local repository, switch to *person-b* branch.

```git
git checkout person-b
```

**12.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-b
```

**13.** On your remote repository, click **Compare & pull request**.

![0008](/images/13/2/0008.svg?featherlight=false&width=100pc)

**14.** Click **Create pull request**.

![0009](/images/13/2/0009.svg?featherlight=false&width=100pc)

**15.** Boom! Your CI check has failed. Click **Details** to explore.

![00010](/images/13/2/00010.svg?featherlight=false&width=100pc)

**16.** Expand the **Show main.py content** dropdown, and you might notice the default value of *n* is now 7. You should recall that person *b* did not modify this in his code changes (see step **15** in [13.1 Prepare Source Code](13-experiments-with-gitHub-actions-merge-group/1-prepare-source-code)).

![00011](/images/13/2/00011.svg?featherlight=false&width=100pc)

It turns out that GitHub Actions Checkout will retrieve the merged code between the pull request and the target (*main*) branch. Learn more about the issuse [here](https://github.com/actions/checkout/issues/881).

This experiment demonstrates that even if your code passes all local tests and shows no conflicts on the pull request, you may still have a chance of CI failures on the remote CI workflow. This can occur if you forget to pull the latest updates from the *main* branch and test them with your local changes to catch any hidden issues before making a pull request.