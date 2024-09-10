---
title : "Get The Prepared Source Code"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 11.2 </b> "
---

**1.** Change into the AWSome Books local repository that you have created in [4.1 Create AWSome Books Repository](4-preparation/1-create-awsome-books-repository).

```git
cd path/to/awsome-books
```

**2.** Clone the following repository [github.com/Definitely-not-AWS-Workshops/workshop-2-awsome-books](https://github.com/Definitely-not-AWS-Workshops/workshop-2-awsome-books).

```git
git clone https://github.com/Definitely-not-AWS-Workshops/workshop-2-awsome-books
```

**3.** Remove **.git** folder in **workshop-2-awsome-books** directory. Copy all files from the **workshop-2-awsome-books** directory to **awsome-books** and then delete the **workshop-2-awsome-books** directory.

```git
rm -rf ./workshop-2-awsome-books/.git && cp -r ./workshop-2-awsome-books/. . && rm -rf workshop-2-awsome-books
```

**4.** Open up the source code.

```git
code .
```

Next, let's go through parts that need to review or change for our CI/CD pipeline.