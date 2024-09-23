---
title : "Configure Database Credentials"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 16.1 </b> "
---

**1.** Make sure you are in the **awsome-books** local repository.

```git
cd path/to/awsome-books
```

**2.** Create and switch to another branch for configuration.

```git
git checkout -b "config-db"
```

**2.** In the *appspec.yml* file, replace the value **\<YOUR-CONTAINER-NAME\>** with `awsome-books` and **\<YOUR-CONTAINER-PORT\>** with `8080` using the command line.

```git
sed -i 's/<YOUR-CONTAINER-NAME>/awsome-books/g' appspec.yml && sed -i 's/<YOUR-CONTAINER-PORT>/8080/g' appspec.yml
```


**3.** In the *src/main/resources/application-prod.yml* file, update **\<YOUR-AWS-SECRECTS-MANAGER-NAME\>** with **\<AWS-SECRECTS-MANAGER-NAME\>** which is the value you recorded in step **14** of [10.2 Create AWS RDS Multi AZ](10-create-aws-rds-resources/2-create-aws-rds-multi-az), and replace **\<YOUR-AWS-RDS-DB-URL\>** with **\<AWS-RDS-DB-URL\>** which is the value noted in step **16** of the same section, using the command line.

```git
sed -i 's/<YOUR-AWS-SECRECTS-MANAGER-NAME>/<AWS-SECRECTS-MANAGER-NAME>/g' src/main/resources/application-prod.yml && sed -i 's/<YOUR-AWS-RDS-DB-URL>/<AWS-RDS-DB-URL>/g' src/main/resources/application-prod.yml
```

**4.** Make another commit and push to the remote corresponding branch.

```git
git add . && git commit -m "configure database credentials" && git push --set-upstream origin config-db
```

**5.** On your remote repository, click **Compare & pull request**.

![0001](/images/16/1/0001.svg?featherlight=false&width=100pc)

**6.** Click **Create pull request**.

![0002](/images/16/1/0002.svg?featherlight=false&width=100pc)