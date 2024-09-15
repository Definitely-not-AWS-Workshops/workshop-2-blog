---
title : "Experiment 2"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 13.3 </b> "
---

You should set up the required code repositories in [13.1 Prepare Source Code](13-experiments-with-gitHub-actions-merge-group/1-prepare-source-code) if you have not done it yet.

In this section, you are going to start by creating a pull request for person *a*'s changes, followed by a separate pull request for person *b*'s. Once both pull requests are up, you will wait for the CI workflows to run. Since person *a*'s changes have not been merged into the *main* branch yet, person *b*'s CI check completes successfully without incorporating person *b*'s updates. Feeling confident, you merge person *b*'s code changes into the *main* branch. But then—boom! The codebase is now broken, even though both pull requests had passed all their tests.

**1.** Make sure you are still in the right project folder.

```git
cd path/to/experiment-2
```

**2.** Switch to *person-a* branch.

```git
git checkout person-a
```

**3.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-a
```

**4.** On your remote repository, click **Compare & pull request**.

![0001](/images/13/3/0001.svg?featherlight=false&width=100pc)

**5.** Click **Create pull request**.

![0002](/images/13/2/0002.svg?featherlight=false&width=100pc)

**6.** On your local repository, switch to *person-b* branch.

```git
git checkout person-b
```

**7.** Push your changes to the remote repository on the corresponding branch.

```git
git push --set-upstream origin person-b
```

**8.** On your remote repository, click **Compare & pull request**.

![0003](/images/13/3/0002.svg?featherlight=false&width=100pc)

**9.** Click **Create pull request**.

![0004](/images/13/2/0009.svg?featherlight=false&width=100pc)

**10.** You should now have passed CI workflow executions of both pull requests. Do not click **Merge pull request** yet.

- Console for the person *a*'s pull request.

![0005](/images/13/3/0003.svg?featherlight=false&width=100pc)

- Console for the person *b*'s pull request.

![0006](/images/13/3/0004.svg?featherlight=false&width=100pc)

**11.** On the person *a*'s pull request console, click **Merge pull request**.

![0007](/images/13/3/0005.svg?featherlight=false&width=100pc)

**12.** Click **Confirm merge**.

![0008](/images/13/3/0006.svg?featherlight=false&width=100pc)

**13.** Click **Delete branch**.

![0009](/images/13/3/0007.svg?featherlight=false&width=100pc)

**14.** Click the **Code** tab and then open the *main.py* file.

![00010](/images/13/3/0008.svg?featherlight=false&width=100pc)

**15.** You might notice that the default value of *n* is now 7.

![00011](/images/13/3/0009.svg?featherlight=false&width=100pc)

**16.** On the person *b*'s pull request console, click **Merge pull request**.

![00012](/images/13/3/00010.svg?featherlight=false&width=100pc)

**12.** Click **Confirm merge**.

![00013](/images/13/3/00011.svg?featherlight=false&width=100pc)

**13.** Click **Delete branch**.

![00014](/images/13/3/00012.svg?featherlight=false&width=100pc)

**14.** Click the **Code** tab and then open the *main.py* file.

![00015](/images/13/3/00013.svg?featherlight=false&width=100pc)

**15.** In addition to the default value of *n* is  7, you might notice that the return value of the function has changed.

Boom!!! Your *main* branch is now broken. Even if each pull request passes its CI checks, code changes made to different lines — without any visible conflicts — can still break your codebase.

![00016](/images/13/3/00014.svg?featherlight=false&width=100pc)

**16.** On your local repository, switch to the *main* branch.

```git
git checkout main
```

**17.** Pull the lasted codes from the remote *main* branch.

```git
git pull
```

**18.** Run the test.

```git
python test.py
```

Your test cases should now fail. 

![00017](/images/13/3/00015.svg?featherlight=false&width=100pc)

Even with local testings and CI workflow executions have passed on individual pull requests, your *main* codebase can still break if it is not tested with the latest merged code on the local machine earlier. To help prevent this, let’s take the next step and create a basic branch protection rule to safeguard your workflow and reduce the chances of errors slipping through.