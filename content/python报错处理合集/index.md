+++
title = "python报错处理合集"
date = 2022-01-15 14:43:14
slug = "202201151443"

[taxonomies]
tags = [ "python" ]
categories = ["python"]

+++

<!-- more -->

## Python版本问题

### ModuleNotFoundError: No module named 'cPickle'

python2有cPickle，但是python3没有cPickle

python2使用：

```python
import cPickle
```

python3使用：

```python
import pickle
```

### name 'reload' is not defined

- 在Python2.x中由于str和byte之间没有明显区别，经常要依赖于defaultencoding来做转换。 
- 在python3中有了明确的str和byte类型区别，从一种类型转换成另一种类型要显式指定*encoding。*

python2使用：

```python
import sys
reload(sys)
sys.setdefaultencoding(‘utf-8’)
```

python3使用：

```python
import importlib,sys
importlib.reload(sys)
```

### AttributeError: module 'sys' has no attribute 'setdefaultencoding'

报错原因同上，python3没有`sys.setdefaultencoding('utf-8')`，删除该行即可



## PyQt5相关

### ERROR: Could not find a version that satisfies the requirement PIL (from versions: none)

ERROR: No matching distribution found for PIL

PIL，全称 Python Imaging Library，是 Python 平台一个功能非常强大而且简单易用的图像处理库。但是，由于 PIL 仅支持到Python 2.7，加上年久失修，于是一群志愿者在 PIL 的基础上创建了兼容 Python 3 的版本，名字叫 Pillow ，现在可以通过安装 Pillow 来使用 PIL。

```python
pip install pillow
```



### ImportError: cannot import name 'options' from 'pyecharts'

解决：

```
pip install pyecharts -U
```



## Numpy相关

### ValueError: numpy.ndarray size changed, may indicate binary incompatibility.

核心原因是 numpy 版本与当前一些库不匹配，把 numpy 更新到最新版本即可

```
pip install --upgrade numpy
```



## urllib相关

### AttributeError: module 'urllib' has no attribute 'request'

解决：

```
from urllib import request
```

### Unresolved reference 'urllib2'

解决：

在python3.3以后，用urllib.request代替urllib2

import语句使用

```
from urllib import request
```

