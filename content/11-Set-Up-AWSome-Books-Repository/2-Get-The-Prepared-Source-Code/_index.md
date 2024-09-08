---
title : "Get The Prepared Source Code"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 11.2 </b> "
---

**1.** Clone the following repository [github.com/Definitely-not-AWS-Workshops/workshop-2-awsome-books](https://github.com/Definitely-not-AWS-Workshops/workshop-2-awsome-books).

```git
git clone https://github.com/Definitely-not-AWS-Workshops/workshop-2-awsome-books
```

Change the directory to the repository you have just cloned.

```git
cd workshop-2-awsome-books
```

**2.** Remove git configuration.

```git
rm -rf .git
```

**3.** Initialize the local git repository.

```git
git init
```

**4.** Create and switch to the *config* branch.

```git
git checkout -b config
```

**5.** Open up the source code.

```git
code .
```

Next, let's go through parts that need to review or change for our CI/CD pipeline.