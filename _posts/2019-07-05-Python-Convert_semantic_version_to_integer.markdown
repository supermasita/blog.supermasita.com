---
layout: post
title:  "Python - Convert semantic version to integer"
date:   2019-07-05 07:04:16 -0300
tags: computers coding python
---
How can I search a range of versions? How can I have a integer representing a semantic version?

Looking for a way to convert semantic version to integer, I found and [answer in **Stack Overflow**](https://stackoverflow.com/questions/41516633/how-to-convert-version-number-to-integer-value#41516650), which I think is the correct one... although it was marked as being so. 

Here is shown as a Python function:
```python
def semantic_to_int(version):
    """
    Convert semantic version to integer, in a way that
    new version have always higher numbers.
    """
    # "5.1.13"
    l = [int(x, 10) for x in version.split('.')]
    # List created from string [5, 1, 13]
    l.reverse()
    # List reversed [13, 1, 5]
    version = sum(x * (100 ** i) for i, x in enumerate(l))
    # Use 'enumarate' to get a tuple with position/index of the list
    # and its value. Multiply position/index by the number: 
    # [13, 100, 50000]
    # Add up all numbers:
    # 50113
    return version
```

### Keep reading
* <https://docs.python.org/3.5/library/functions.html#enumerate>

