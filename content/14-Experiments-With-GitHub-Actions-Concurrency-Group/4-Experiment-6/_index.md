---
title : "Experiment 6"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 14.4 </b> "
---

**1.** Make sure you are still in the right project folder.

```git
cd path/to/experiment-5-6-7
```

**2.** In the *.github/workflows/main.yml* file, add *concurrency group* at the workflow level.

```yml {linenos=table,hl_lines=["7-8"],linenostart=1}
name: main

on:
  push:
    branches: ["main"]

concurrency:
  group: main

jobs:
  run-test:
    runs-on: ubuntu-latest
    steps:
      - name: Extend the workflow duration by an additional 300 seconds.
        run: sleep 300
```

**3.** Modify the *README.md* file with the following content.

```git
echo "experiment 6: the first workflow execution!" > README.md
```

**4.** Make another commit and push to the remote repository.

```git
git add . && git commit -m "experiment 6: the first workflow execution" && git push
```

**5.** On your remote repository, click the **Actions** tab, you should see that a workflow execution has been triggered.

![0001](/images/14/4/0001.svg?featherlight=false&width=100pc)

**6.** On your local repository, update the *README.md* file with the following content.

```git
echo "experiment 6: the second workflow execution!" > README.md
```

**7.** Make another commit and push to the remote repository.

```git
git add . && git commit -m "experiment 6: the second workflow execution" && git push
```

**8.** On your remote repository, under the **Actions** tab, while the first workflow execution is running, you may notice that the second workflow execution is in the **Pending** state.

![0002](/images/14/4/0002.svg?featherlight=false&width=100pc)

**9.** On your local repository, update the *README.md* file with the following content.

```git
echo "experiment 6: the third workflow execution!" > README.md
```

**10.** Make another commit and push to the remote repository.

```git
git add . && git commit -m "experiment 6: the third workflow execution" && git push
```

**11.** On your remote repository, under the **Actions** tab, while the first workflow execution is running, you may notice that the second workflow execution is canceled and the third workflow execution is in **Pending** state.

![0003](/images/14/4/0003.svg?featherlight=false&width=100pc)

As you can see, GitHub Actions enables multiple workflow runs to occur simultaneously by default, which can lead to the drawbacks we discussed in [14.1 You Might Want A Single Workflow Execution At A Time!](14-experiments-with-gitHub-actions-concurrency-group/1-you-might-want-a-single-workflow-execution-at-a-time!).






