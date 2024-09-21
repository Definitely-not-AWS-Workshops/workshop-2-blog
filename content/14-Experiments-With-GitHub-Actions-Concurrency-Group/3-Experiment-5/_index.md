---
title : "Experiment 5"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 14.3 </b> "
---

In this section, let’s experiment by running two workflow executions without *concurrency group*. Let’s find out what happens!

**1.** Make sure you are still in the right project folder.

```git
cd path/to/experiment-5-6-7
```

**2.** Add the following content to *README.md* file.

```git
echo "experiment 5: the first workflow execution!" > README.md
```

**3.** Make your first commit and push to the remote repository.

```git
git add . && git commit -m "experiment 5: the first workflow execution" && git push --set-upstream origin main
```

**4.** On your remote repository, click the **Actions** tab, you should see that a workflow execution has been triggered.

![0001](/images/14/3/0001.svg?featherlight=false&width=100pc)

**5.** On your local repository, update the *README.md* file with the following content.

```git
echo "experiment 5: the second workflow execution!" > README.md
```

**6.** Make another commit and push to the remote repository.

```git
git add . && git commit -m "experiment 5: the second workflow execution" && git push
```

**7.** On your remote repository, under the **Actions** tab, while the first workflow execution is running, you may notice that another workflow has been triggered and is now running as well.

![0002](/images/14/3/0002.svg?featherlight=false&width=100pc)

As you can see, GitHub Actions enables multiple workflow runs to occur simultaneously by default, which can lead to the drawbacks we discussed in [14.1 You Might Want A Single Workflow Execution At A Time!](14-experiments-with-gitHub-actions-concurrency-group/1-you-might-want-a-single-workflow-execution-at-a-time!).






