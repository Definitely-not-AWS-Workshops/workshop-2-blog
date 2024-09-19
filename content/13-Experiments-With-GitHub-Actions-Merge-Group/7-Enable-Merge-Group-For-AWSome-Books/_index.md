---
title : "Enable Merge Group For AWSome Books"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 13.7 </b> "
---

Since the CI workflow of AWSome Books has been set up with the **merge_group** event (checkout the *.github/workflows/ci.yml* file), 

```yml {linenos=table,hl_lines=["11-12"],linenostart=1}
name: CI

on:
  pull_request:
    branches: [main]
    types: [opened, synchronize, reopened]
    paths-ignore:
      - docs/**
      - docker-compose.dev.yml

  merge_group:
    branches: [main]

  workflow_dispatch:
```

you just need to enable the **Require merge queue** rule in the ruleset.

**1.** Under the **awsome-books** GitHub repository,

- Click **Settings**.
- Expand the **Rules** dropdown.
- Select **Rulesets**.
- Click **main-protection**.

![0001](/images/13/7/0001.svg?featherlight=false&width=100pc)

**2.** Enable **Require merge queue**.

![0002](/images/13/6/0003.svg?featherlight=false&width=100pc)


**3.** Scroll down to the bottom, click **Save changes**.

![0003](/images/13/7/0002.svg?featherlight=false&width=100pc)

Now, the AWSDSC-XUT team can accelerate code integration into a busy branch, while ensuring it remains stable and free from incompatible changes.