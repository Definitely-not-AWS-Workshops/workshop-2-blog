---
title : "Configure Database Credentials"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 16.1 </b> "
---

The necessary database credentials must first be configured before the AWSome Books project is released.

**1.** Make sure you are in the **awsome-books** local repository.

```git
cd path/to/awsome-books
```

**2.** Create and switch to another branch for configuration.

```git
git checkout -b "config-db"
```

**3.** In the *appspec.yml* file, replace the value **\<YOUR-CONTAINER-NAME\>** with `awsome-books` and **\<YOUR-CONTAINER-PORT\>** with `8080` using the command line.

```git
sed -i 's/<YOUR-CONTAINER-NAME>/awsome-books/g' appspec.yml && sed -i 's/<YOUR-CONTAINER-PORT>/8080/g' appspec.yml
```

**4.** In the *src/main/resources/application-prod.yml* file, update **\<YOUR-AWS-SECRECTS-MANAGER-NAME\>** with **\<AWS-SECRECTS-MANAGER-NAME\>** which is the value you recorded in step **14** of [10.2 Create AWS RDS Multi AZ](10-create-aws-rds-resources/2-create-aws-rds-multi-az), and replace **\<YOUR-AWS-RDS-DB-URL\>** with **\<AWS-RDS-DB-URL\>** which is the value noted in step **16** of the same section, using the command line.

```git
sed -i 's/<YOUR-AWS-SECRECTS-MANAGER-NAME>/<AWS-SECRECTS-MANAGER-NAME>/g' src/main/resources/application-prod.yml && sed -i 's/<YOUR-AWS-RDS-DB-URL>/<AWS-RDS-DB-URL>/g' src/main/resources/application-prod.yml
```

**5.** Make another commit and push to the remote corresponding branch.

```git
git add . && git commit -m "configure database credentials" && git push --set-upstream origin config-db
```

**6.** On your remote repository, click **Compare & pull request**.

![0001](/images/16/1/0001.svg?featherlight=false&width=100pc)

**7.** Click **Create pull request**.

![0002](/images/16/1/0002.svg?featherlight=false&width=100pc)

**8.** A notification indicates the CI workflow is triggered via the pull request sent to the **gha-ci** Slack channel. 

![0003](/images/16/1/0003.svg?featherlight=false&width=100pc)

After a while, you might notice that the CI success notification has been sent to the channel.

![0004](/images/16/1/0004.svg?featherlight=false&width=100pc)

**9.** Back to the pull request details console, click **Merge when ready**.

![0005](/images/16/1/0005.svg?featherlight=false&width=100pc)

**10.** Click **Confirm merge when ready**.

![0006](/images/16/1/0006.svg?featherlight=false&width=100pc)

**11.** You may receive a notification in the **gha-merge-group** channel indicating that the *merge-group* event's CI workflow has begun.

![0007](/images/16/1/0007.svg?featherlight=false&width=100pc)

Give it a few minutes, and your workflow should be executed successfully. 

![0008](/images/16/1/0008.svg?featherlight=false&width=100pc)