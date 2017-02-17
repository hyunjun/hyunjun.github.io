---
layout: post
title: Python, print unicode character in window
---

* Mac

  ```
$ /path/to/anaconda3/bin/python
Python 3.5.2 |Anaconda 4.2.0 (x86_64)| (default, Jul  2 2016, 17:52:12)
[GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> a = '\u2603'
>>> print(a)
☃
>>>
  ```

* Linux

  ```
$ /path/to/anaconda3/bin/python
Python 3.5.1 |Anaconda 4.0.0 (64-bit)| (default, Dec  7 2015, 11:16:01)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> a = '\u2603'
>>> print(a)
☃
>>>
  ```

* Windows10
  * ![python window unicode]({{ site.baseurl }}/images/python_window_unicode.png)
  * [`chcp 65001`](http://stackoverflow.com/questions/388490/unicode-characters-in-windows-command-line-how/388500#388500)
