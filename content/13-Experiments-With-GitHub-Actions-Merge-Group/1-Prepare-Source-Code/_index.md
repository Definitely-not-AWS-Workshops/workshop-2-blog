---
title : "Prepare Source Code"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 13.1 </b> "
---

Please begin by reading the following subsections, **Experiment-n** (where *n* ranges from **1** to **4**). You then return to this section to complete the codebase setup for each section.

**1.** Go to your GitHub organization created in [4.1 Create AWSome Books Repository](4-Preparation/1-Create-AWSome-Books-Repository).

- Click the dropdown.
- Select **New repository**.

![0001](/images/13/1/0001.svg?featherlight=false&width=100pc)

**2.** Fill out the following information.

- For **Owner**, choose `fcj-workshops-2024`.
- For **Repository name**, enter `experiment-n` where *n* is the experimental subsection you are working on.
- Select **Public** option.

![0002](/images/13/1/0002.svg?featherlight=false&width=100pc)

**3.** Choose your favorite workspace on your local machine. Clone this remote repository [Definitely-not-AWS-Workshops/get-1s](https://github.com/Definitely-not-AWS-Workshops/get-1s).

```git
git clone https://github.com/Definitely-not-AWS-Workshops/get-1s
```

**4.** Change the directory name to `experiment-n` where *n* is the experimental subsection you are working on.

```git
mv get-1s experiment-n
```

**5.** Change into the directory `experiment-n` where *n* is the experimental subsection you are working on. And then remove the *.git* folder.

```git
cd experiment-n && rm -rf .git
```

**6.** Initialize a new git repository.

```git
git init
```

**7.** Link to the remote repository that you have just created with `experiment-n` where *n* is the experimental subsection you are working on

```git
git remote add origin https://github.com/fcj-workshops-2024/experiment-n.git
```

**8.** Make the first commit and push to the remote repository.

```git
git add . && git commit -m "first commit" && git push --set-upstream origin main
```

**9.** Create and switch to the *person-a* branch.

```git
git checkout -b person-a
```

**10.** In the *main.py* file, change the default value of the parameter *n* from `DEFAULT` to `7`.

```python {linenos=table,hl_lines=["3-4"],linenostart=1}
DEFAULT=1

- def get_1s(n=DEFAULT):
+ def get_1s(n=7):

    '''
        TODO: return an array of 1s
    '''

    return [1] * n
```

**11.** Run the test.

```git
python test.py
```

It should be successful for your test to run.

![0003](/images/13/1/0003.svg?featherlight=false&width=100pc)

**12.** Make a commit.

```git
git add . && git commit -m "change the default value of the parameter n from DEFAULT to 7"
```

**13.** Switch to the *main* branch.

```git
git checkout main
```

**14.** Create and switch to the *person-b* branch.

```git
git checkout -b person-b
```

**15.** Make a change to *main.py* file.

```python {linenos=table,hl_lines=["9-10"],linenostart=1}
DEFAULT=1

def get_1s(n=DEFAULT):

    '''
        TODO: return an array of 1s
    '''

-   return [1] * n
+   return [1] * (30 if n==DEFAULT else n)
```

**16.** Add a test to *test.py* file.

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

**17.** Run the test.

```git
python test.py
```

It should be successful for your test to run.

![0004](/images/13/1/0004.svg?featherlight=false&width=100pc)

**18.** Make a commit.

```git
git add . && git commit -m "modify the get_1s return and add another test"
```