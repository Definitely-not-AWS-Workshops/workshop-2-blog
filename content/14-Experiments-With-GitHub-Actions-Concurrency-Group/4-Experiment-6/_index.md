---
title : "Experiment 6"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 14.4 </b> "
---

Now, you add a *concurrency group* but have not enabled **cancel-in-progress**, which would automatically cancel any currently running workflows within the same *concurrency group*.

**1.** Make sure you are still in the right project folder.

```git
cd path/to/experiment-5-6-7
```

**2.** In the *.github/workflows/main.yml* file,

- Add *concurrency group* at the workflow level to ensure that only a single workflow using the same *concurrency group* will run at a time.
  
- With a group named **main**, when a workflow is queued, it will be in the pending state if a different workflow using the **main** group is already in progress. If another workflow enters the queue, any pending workflow in the group will be canceled. This ensures that at any given time, there can be a maximum of one running job and one pending job in the group.

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

**11.** On your remote repository, under the **Actions** tab, as the first workflow runs, you might notice the second workflow gets canceled, while the third goes in a **Pending** state, waiting for its turn.

![0003](/images/14/4/0003.svg?featherlight=false&width=100pc)

A *merge group* without **cancel-in-progress** enabled ensures that, at any moment, there can be no more than one running job and one pending job within the group.





