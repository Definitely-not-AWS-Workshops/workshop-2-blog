---
title : "Prepare Source Code"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 13.2 </b> "
---

Please begin by reading the following subsections, *Experiment n* (where *n* ranges from **1** to **4**). You then return to this section to complete the codebase setup for each section.

**1.** Go to your GitHub organization created in [4.1 Create AWSome Books Repository](4-Preparation/1-Create-AWSome-Books-Repository).

- Click the dropdown.
- Select **New repository**.

![0001](/images/13/2/0001.svg?featherlight=false&width=100pc)

**2.** Fill out the following information.

- For **Owner**, choose `fcj-workshops-2024`.
- For **Repository name**, enter `experiment-n` where *n* is the experimental subsection you are working on.
- Select **Public** option.

![0002](/images/13/2/0002.svg?featherlight=false&width=100pc)

**3.** Scroll down to the botttom. Click **Create repository**.

![00010](/images/4/1/00010.svg?featherlight=false&width=100pc)

**4.** Choose your favorite workspace on your local machine. Clone this remote repository [github.com/Definitely-not-AWS-Workshops/get-1s](https://github.com/Definitely-not-AWS-Workshops/get-1s).

```git
git clone https://github.com/Definitely-not-AWS-Workshops/get-1s
```

Along with the unchanged content of *main.py* and *test.py* from the previous section, let's now get an overview of the CI workflow defined in *.github/workflows/ci.yml*.

```yml
name: CI

on:
  pull_request:

  merge_group:
    branches: [main]

jobs:
  run-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.9"

      - name: Extend the workflow duration by an additional 10 seconds.
        run: sleep 10

      - name: Show main.py content
        run: cat main.py

      - name: Show test.py content
        run: cat test.py

      - name: Run tests
        run: python test.py
```

In general, this workflow is triggered when a pull request is created or when a branch is part of a *merge group* (*main* branch). It sets up a CI job on an Ubuntu machine, checks out the code, installs Python 3.9, prints the contents of two files (*main.py* and *test.py*) for troubleshooting, and finally runs the test file (*test.py*) to validate the code changes. The sleep 10 step appears to add a delay, possibly for debugging purposes.

**5.** Change the directory name to `experiment-n` where *n* is the experimental subsection you are working on.

```git
mv get-1s experiment-n
```

**6.** Change into the directory `experiment-n` where *n* is the experimental subsection you are working on. And then remove the *.git* folder.

```git
cd experiment-n && rm -rf .git
```

**7.** Initialize a new git repository.

```git
git init
```

**8.** Link to the remote repository that you have just created with `experiment-n` where *n* is the experimental subsection you are working on

```git
git remote add origin https://github.com/fcj-workshops-2024/experiment-n.git
```

**9.** Make your first commit and push to the remote repository.

```git
git add . && git commit -m "first commit" && git push --set-upstream origin main
```

**10.** Create and switch to the *person-a* branch.

```git
git checkout -b person-a
```

**11.** In the *main.py* file, change the default value of the parameter *n* from `DEFAULT` to `7`.

```python {linenos=table,hl_lines=["3-4"],linenostart=1}
DEFAULT=1

- def get_1s(n=DEFAULT):
+ def get_1s(n=7):

    '''
        TODO: return an array of 1s
    '''

    return [1] * n
```

**12.** Run the test.

```git
python test.py
```

It should be successful for your test to run.

![0003](/images/13/2/0003.svg?featherlight=false&width=100pc)

**13.** Make a commit.

```git
git add . && git commit -m "change the default value of the parameter n from DEFAULT to 7"
```

**14.** Switch to the *main* branch.

```git
git checkout main
```

**15.** Create and switch to the *person-b* branch.

```git
git checkout -b person-b
```

**16.** Make a change to *main.py* file.

```python {linenos=table,hl_lines=["9-10"],linenostart=1}
DEFAULT=1

def get_1s(n=DEFAULT):

    '''
        TODO: return an array of 1s
    '''

-   return [1] * n
+   return [1] * (30 if n==DEFAULT else n)
```

**17.** Add a test to *test.py* file.

```python {linenos=table,hl_lines=["12-16"],linenostart=1}
import unittest
from main import get_1s

class TestGet1sFunction(unittest.TestCase):
    
    def test_with_10(self):
        """Test the function with 10."""
        result = get_1s(10)
        expected_result = [1] * 10
        self.assertEqual(result, expected_result)

+   def test_with_default(self):
+       """Test the function with default."""
+       result = get_1s()
+       expected_result = [1] * 30
+       self.assertEqual(result, expected_result)

if __name__ == "__main__":
    unittest.main()

```

**18.** Run the test.

```git
python test.py
```

It should be successful for your test to run.

![0004](/images/13/2/0004.svg?featherlight=false&width=100pc)

**19.** Make a commit.

```git
git add . && git commit -m "modify the get_1s return and add another test"
```

Your code changes on each branch have passed all local tests successfully! However, if either person *a* or person *b* merges their changes into the *main* branch first, and on the other branch you then forget to pull the latest updates from *main* before running your local tests again, you might run into unexpected issues when creating a pull request and merging back to *main*.

To investigate, you run a series of experiments to see if your well-tested local code changes could still fail during pull requests, even when there are no visible code conflicts.