---
layout: post
title: "Some questions you may be asked during a Python job interview."
date: 2014-06-14 00:28
categories: Programming
tags:
  - Python
  - Interview
slug: python-interview-questions
---

It's fairly easy to learn the Python syntax, but Python as a full-featured language, is not that easy to be fully mastered.

In my experience, these questions are the most likely be asked during a job interview about python, I wrote down the answers, hope that may help you!

![Python](http://i.imgur.com/PO4oXF1.png)

**Q: What is *lambda operator* in Python, how do you use it?**

A: The lambda operator or lambda function is a way to create small anonymous functions

for example:

<pre class="brush: python; highlight: python; html-script: true">
>>> f = lambda x, y : x + y
>>> f(2,2)
4
</pre>

**Q: What is PEP8?**

A: PEP8(Python Enhancement Proposal) is a coding standard proposed by Guido and others.

<!--more-->

**Q: What is the differences between List and Set in Python?**

A: Set contains **unordered** collections of **unique**  elements, List contains ordered collections of elements. In a Set, Python uses hashtable to find elements, which is more efficient than iterating through all the elements in a List. So Set can only contains hashable items.

**Q: How are arguments passed – by reference or by value?**

A: Nither, Here is a good article to explain it: [Is Python call-by-value or call-by-reference? Neither.](http://www.jeffknupp.com/blog/2012/11/13/is-python-callbyvalue-or-callbyreference-neither/)

**Q: What is GIL in Python?**

A: Python's GIL is intended to serialize access to interpreter internals from different threads. On multi-core systems, it means that multiple threads can't effectively make use of multiple cores. (If the GIL didn't lead to this problem, most people wouldn't care about the GIL - it's only being raised as an issue because of the increasing prevalence of multi-core systems.)

ref: [What is a global interpreter lock (GIL)?](http://stackoverflow.com/questions/1294382/what-is-a-global-interpreter-lock-gil)

**Q: Given `a = [1,5,3]`, what is the difference between `a.sort()` and `sorted(a)`?**

A: `sorted()` is a built-in function which sort the list `a` and return it, while `sort()` is an function under `a` which return nothing.

**Q: What is the difference between `_xx`, ` __xx` and `__xx__` in Python?**

A: The first one and the third one are conventions, the second one has a real meaning. One underline in the beginning of a method or attribute means you shouldn't access this method. Two underline in the beginning of a method tell the interpreter to replace this name with `_classname__xx` as a way to ensure that the name will not overlap with a similar name in another class. Two underlines in the beginning and in the end means these names are used by Python system. This article explains everything: [Difference between _, __ and __xx__ in Python ](http://igorsobreira.com/2010/09/16/difference-between-one-underline-and-two-underlines-in-python.html)

**Q: What is the difference between `class Foo:` and `class Foo(object):` in Python?**

A: We call `class Foo:` old-style class, and `class Foo(object):` is called new-style class. They have different object models, and new-style class has `super()`, `@property` and `descriptors`, etc, wthich can not be found in old-style classes. See this article for a good description of what a new style class is: [Type and Class Changes](https://docs.python.org/release/2.2.3/whatsnew/sect-rellinks.html)

**TO BE CONTINUED**

---

If you find out anything wrong, please leave a comment.
