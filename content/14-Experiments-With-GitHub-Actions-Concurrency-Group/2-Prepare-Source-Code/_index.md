---
title : "Prepare Source Code"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 14.2 </b> "
---

**1.** Go to your GitHub organization created in [4.1 Create AWSome Books Repository](4-Preparation/1-Create-AWSome-Books-Repository).

- Click the dropdown.
- Select **New repository**.

![0001](/images/13/2/0001.svg?featherlight=false&width=100pc)

**2.** Fill out the following information.

- For **Owner**, choose `fcj-workshops-2024`.
- For **Repository name**, enter `experiment-5-6-7` 
- Select **Public** option.

![0002](/images/14/2/0001.svg?featherlight=false&width=100pc)

**3.** Scroll down to the botttom. Click **Create repository**.

![0003](/images/4/1/00010.svg?featherlight=false&width=100pc)

**4.** Choose your favorite workspace on your local machine. Next, create and move to the experiment folder.

```git
mkdir experiment-5-6-7 && cd experiment-5-6-7
```

**5.** Initialize the local git repository.

```git
git init
```

**6.** Link to the remote repository that you have just created.

```git
git remote add origin https://github.com/fcj-workshops-2024/experiment-5-6-7.git
```

**7.** Create *.github/workflows/main.yml* file.

```git
mkdir -p .github/workflows && touch .github/workflows/main.yml
```

**8.** Add the following content to *.github/workflows/main.yml* file:

```yml
name: main

on:
  push:
    branches: ["main"]

jobs:
  run-test:
    runs-on: ubuntu-latest
    steps:
      - name: Extend the workflow duration by an additional 300 seconds.
        run: sleep 300
```

Overall, this workflow serves a very basic purpose: it will trigger whenever there is a push to the *main* branch and will extend the execution time by 300 seconds for testing or to simulate a longer running job.

Next, let’s first dive into the fifth experiment without utilizing the *concurrency group*.