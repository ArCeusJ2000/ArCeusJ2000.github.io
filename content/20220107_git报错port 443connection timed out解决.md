+++
title = "git报错port 443:connection timed out解决"
date = 2022-01-07 17:45:12
slug = "202201071745"

[taxonomies]
tags = ["git"]
categories = ["git"]

+++

<!-- more -->

注意，该方法不一定每次都有效orz

## 报错内容

```
Failed to connect to github.com port 443:connection timed out
```

git clone / pull / push都有可能出现，



## 比较可能有用的解决方法

### 重新设置代理

```
$ git config --global http.proxy http://127.0.0.1:1080

$ git config --global https.proxy http://127.0.0.1:1080
```



### 取消全局代理

```
$ git config --global --unset http.proxy
 
$ git config --global --unset https.proxy
```

体感上这个有用的概率更高一点



## 其他曾经有效的解决方法

#### 包括但不限于：

把梯子关了

重开一个git bash

关机重启

换个时间再试一次
