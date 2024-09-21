---
title : "Experiment 7"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 14.5 </b> "
---

There are times when you may need to cancel any currently running workflow within the same *concurrency group*. To do this, simply set **cancel-in-progress: true**.

**1.** Make sure you are still in the right project folder.

```git
cd path/to/experiment-5-6-7
```

**2.** In the *.github/workflows/main.yml* file, add `cancel-in-progress: true` setting for *concurrency group*.

```yml {linenos=table,hl_lines=[9],linenostart=1}
name: main

on:
  push:
    branches: ["main"]

concurrency:
  group: main
  cancel-in-progress: true

jobs:
  run-test:
    runs-on: ubuntu-latest
    steps:
      - name: Extend the workflow duration by an additional 300 seconds.
        run: sleep 300
```

**3.** Modify the *README.md* file with the following content.

```git
echo "experiment 7: the first workflow execution!" > README.md
```

**4.** Make another commit and push to the remote repository.

```git
git add . && git commit -m "experiment 7: the first workflow execution" && git push
```

**5.** On your remote repository, click the **Actions** tab, you should see that a workflow execution has been triggered.

![0001](/images/14/5/0001.svg?featherlight=false&width=100pc)

**6.** On your local repository, update the *README.md* file with the following content.

```git
echo "experiment 7: the second workflow execution!" > README.md
```

**7.** Make another commit and push to the remote repository.

```git
git add . && git commit -m "experiment 7: the second workflow execution" && git push
```

**8.** On your remote repository, under the **Actions** tab, you may notice that the first workflow execution has been canceled, while the second workflow is now running.

![0002](/images/14/5/0002.svg?featherlight=false&width=100pc)

Throughout the experiments, leveraging a *concurrency group* proves beneficial, allowing you to run a single workflow at a time. This is particularly useful for managing your account's or organization's resources, especially in situations where running multiple workflows simultaneously could lead to conflicts or unexpectedly high consumption of Actions minutes and storage.





