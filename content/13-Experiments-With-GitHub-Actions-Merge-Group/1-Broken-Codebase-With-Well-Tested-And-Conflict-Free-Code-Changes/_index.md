---
title : "Broken Codebase With Well-Tested And Conflict-Free Code Changes"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 13.1 </b> "
---

The following scenario assumes a situation where even well-tested, conflict-free code changes from other branches can unexpectedly disrupt the *main* branch.

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

This *main.py* script defines a constant *DEFAULT* with a value of 1 and a function *get_1s(n=DEFAULT)*. The function returns an array (list) of 1s, where the length of the array is determined by the parameter *n*. If no argument is provided, the default value for *n* is 1, meaning it will return a list with one element: *[1]*.

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

This *test.py* file uses the *unittest* framework to test the *get_1s* function. The *test_with_10* method checks if calling *get_1s(10)* returns a list of ten *1*s by comparing the result with *[1] * 10*. If the outputs match, the test passes. The script runs the test when executed directly.

Both developers begin by pulling the latest changes from the *main* branch. From the *main* branch, person *a* creates a new branch, naming it *person-a*, while person *b* checks out a separate branch called *person-b*.

**'person-a' branch**

```python {linenos=table,hl_lines=["3-4"],linenostart=1}
DEFAULT=1

- def get_1s(n=DEFAULT):
+ def get_1s(n=7):

    '''
        TODO: return an array of 1s
    '''

    return [1] * n
```

**'person-b' branch**

```python {linenos=table,hl_lines=["9-10"],linenostart=1}
DEFAULT=1

def get_1s(n=DEFAULT):

    '''
        TODO: return an array of 1s
    '''

-   return [1] * n
+   return [1] * (30 if n==DEFAULT else n)
```

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

Next, let’s conduct a series of experiments before enabling the merge group for AWSome Books.