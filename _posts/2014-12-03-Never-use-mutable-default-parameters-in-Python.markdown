---
layout: post
title: "Never use mutable default parameters in Python"
date: 2014-09-09 00:04
categories: Programming
tags: Python
slug: mutable-default-parameters
---

![Python](/images/python.png)

While writing a Python function, you may need to declare some default parameters, people new to Python may write code like this:

```python
def test(q=[]):
    q.append(1)
    return q
```

This function seems to be well written, whereas when calling it without a given parameter `q`, you may run into trouble.

<!--more-->

```python
>>> test()
[1]
>>> test()
[1, 1]
>>> test()
[1, 1, 1]
>>> test()
[1, 1, 1, 1]
```

This is weird at first, as required the function should return the list `[1]` after calling without a give parameter. However after carefully examining the code, we found that all of the returned list have the same ids.

```python
>>> id(test())
4508921728
>>> id(test())
4508921728
>>> id(test())
4508921728
```

This means that all these function calls are using the same object(the default parameter `q`), for that reason they are always calling `append` method from the same list, and returns it.

## Default Parameters in Python must be immutable

Everything is an object in Python, including functions, and default parameter in function is like a member variable in an object, when the definition of the function is executed, the function object is initiated, and the default parameter values(`[]`) are evaluated and the default parameters(`q`) is a reference to the parameter object.

```python
>>> from datetime import datetime
>>> def test_time(q=datetime.now()):
...     return q
...
>>> test_time()
datetime.datetime(2014, 9, 8, 23, 58, 23, 700633)
>>> test_time()
datetime.datetime(2014, 9, 8, 23, 58, 23, 700633)
>>> test_time()
datetime.datetime(2014, 9, 8, 23, 58, 23, 700633)
```

In the above example, q is established along with the definition code of the function `test_time` being executed, you may notice that they are always returning the same time(time the definition code of the function `test_time` being executed at) instead of the current time. For that reason mutable default parameters are not allowed in Python, since you are operate on the same object when doing changes on them:
