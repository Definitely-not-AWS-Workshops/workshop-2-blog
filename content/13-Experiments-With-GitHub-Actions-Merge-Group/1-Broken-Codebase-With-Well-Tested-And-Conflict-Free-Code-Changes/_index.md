---
title : "Broken Codebase With Well-Tested And Conflict-Free Code Changes"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 13.1 </b> "
---

The following scenario assumes a situation where even well-tested, conflict-free code changes from other branches can unexpectedly disrupt the *main* branch.

{{% notice note %}}
The following scenario is purely for illustration purposes and does not adhere to any engineering practices.
{{% /notice %}}

Imagine two developers, person *a* and person *b*, working together on a shared codebase. The *main* branch currently contains the following files.

Here’s the content of the *main.py* file:

```python
DEFAULT=1

def get_1s(n=DEFAULT):

    '''
        TODO: return an array of 1s
    '''

    return [1] * n
```

This *main.py* script defines a constant **DEFAULT** with a value of **1** and a function **get_1s(n=DEFAULT)**. The function returns an array (list) of 1s, where the length of the array is determined by the parameter **n**. If no argument is provided, the default value for **n** is **1**, meaning it will return a list with one element: **[1]**.

And the content of *test.py* file:

```python
import unittest
from main import get_1s

class TestGet1sFunction(unittest.TestCase):
    
    def test_with_10(self):
        """Test the function with 10."""
        result = get_1s(10)
        expected_result = [1] * 10
        self.assertEqual(result, expected_result)

if __name__ == "__main__":
    unittest.main()
```

This *test.py* file uses the *unittest* framework to test the **get_1s** function. The **test_with_10** method checks if calling **get_1s(10)** returns a list of ten **1**s by comparing the result with **[1] * 10**. If the outputs match, the test passes.  

Both developers begin by pulling the latest codebase from the *main* branch. From the *main* branch, person *a* creates a new branch, naming it *person-a*, while person *b* checks out a separate branch called *person-b*.

#### On 'person-a' branch

In the *main.py* file, person **a** updates the default value of the parameter **n**, changing it from **DEFAULT** to **7**. 

```python {linenos=table,hl_lines=["3-4"],linenostart=1}
DEFAULT=1

- def get_1s(n=DEFAULT):
+ def get_1s(n=7):

    '''
        TODO: return an array of 1s
    '''

    return [1] * n
```

The contents of *test.py* should remain untouched.

```python
import unittest
from main import get_1s

class TestGet1sFunction(unittest.TestCase):
    
    def test_with_10(self):
        """Test the function with 10."""
        result = get_1s(10)
        expected_result = [1] * 10
        self.assertEqual(result, expected_result)

if __name__ == "__main__":
    unittest.main()
```

Running the test should result in a successful pass for the **test_with_10** case.

#### On 'person-b' branch

Meanwhile, on the *person-b* branch, person *b* is also working on the *main.py* file, updating the return value of the **get_1s** function.

```python {linenos=table,hl_lines=["9-10"],linenostart=1}
DEFAULT=1

def get_1s(n=DEFAULT):

    '''
        TODO: return an array of 1s
    '''

-   return [1] * n
+   return [1] * (30 if n==DEFAULT else n)
```

In addition, he adds a new test case in the *test.py* file to validate the use of the default value.

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

Person *b* then runs the tests, successfully passing both the new and existing test cases.

#### Conflicts are not Always Caught

Both developers have modified the same function, **get_1s**, so you might expect the version control system to flag this as a conflict during the merge, right?

Unfortunately, that is not the case! Most version control systems use a fairly basic method for detecting conflicts — they simply look at the lines of code that were changed, without understanding the logic behind those changes. As long as two people edit different lines, the system assumes everything is fine. So, even though both person *a* and person *b* made separate changes to **get_1s**, because they touched different lines, the version control system will merge their changes without a hitch — no conflict detected!

Person *a* merges his changes first, changing the state of **get_1s** in the *main* branch to have his new defaulting logic:

```python {linenos=table,hl_lines=[3],linenostart=1}
DEFAULT=1

def get_1s(n=7):

    '''
        TODO: return an array of 1s
    '''

    return [1] * n
```

After that, person *b* merges his changes in as well. Since person *a*’s changes are already present in the *main* branch, the resulting function should be:

```python {linenos=table,hl_lines=[3,9],linenostart=1}
DEFAULT=1

def get_1s(n=7):

    '''
        TODO: return an array of 1s
    '''

    return [1] * (30 if n==DEFAULT else n)
```

And the content of *test.py* file should be:

```python {linenos=table,hl_lines=["12-16"],linenostart=1}
import unittest
from main import get_1s

class TestGet1sFunction(unittest.TestCase):
    
    def test_with_10(self):
        """Test the function with 10."""
        result = get_1s(10)
        expected_result = [1] * 10
        self.assertEqual(result, expected_result)

    def test_with_default(self):
        """Test the function with default."""
        result = get_1s()
        expected_result = [1] * 30
        self.assertEqual(result, expected_result)

if __name__ == "__main__":
    unittest.main()
```

Oops! The *main* branch is now broken. If you run the test, you will find that the **test_with_default** case fails. Until now, you should have seen that even well-tested and conflict-free code changes can still pose a risk of breaking your codebase.

Running CI triggered by pull requests is a fantastic way to catch these kinds of bugs before they reach the *main* branch (as you will see in the upcoming experimental subsections). However, there is a case where CI might not catch the bug, potentially leading to a broken codebase.

Next, let’s run a series of different experiments to simulate the scenario described in this section and understand why you might need a merge group (or merge queue). After that, while simulating this feature at scale can be challenging, you will conduct a simplified experiment with it and then proceed to just enable the merge group for AWSome Books.