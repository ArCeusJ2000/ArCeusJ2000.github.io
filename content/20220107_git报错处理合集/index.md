+++
title = "git报错处理合集"
date = 2022-01-07 17:45:12
slug = "202201071745"

[taxonomies]
tags = ["git"]
categories = ["git"]

+++

<!-- more -->

大部分都是网络问题报错

又是饱受gfw折磨的一天 テ_デ

记录一下（曾经可能）有效的方法



# 网络问题报错

## 解决github连不上

#### 修改hosts

在 C:\Windows\System32\drivers\etc 文件夹下找到hosts文件

Run  as Administrator打开

添加：

```
140.82.113.3  www.github.com
```

查询IP地址使用：

<https://ipaddress.com>



#### 使用镜像

<https://github.com.cnpmjs.org>



#### 去Gitee碰运气

<https://gitee.com>



#### 科学上网

能解决浏览器访问的问题

但命令行还是会间歇性失效orz

命令行设置代理

```bash
set http_proxy=http://127.0.0.1:1080
set https_proxy=http://127.0.0.1:1080
```



## Failed to connect to github.com port 443:connection timed out

### 比较可能有用的解决方法

#### 重新设置代理

```bash
$ git config --global http.proxy http://127.0.0.1:1080

$ git config --global https.proxy http://127.0.0.1:1080
```



#### 取消全局代理

```bash
$ git config --global --unset http.proxy
 
$ git config --global --unset https.proxy
```

体感上这个有用的概率更高一点



#### 其他曾经有效的解决方法

##### 包括但不限于：

把梯子关了

重开一个git bash

关机重启

换个时间再试一次



## OpenSSL SSL_read: Connection was reset, errno 10054

### OpenSSL SSL_read: Connection was reset, errno 10054 fatal: expected flush after ref listing

```bash
$ git config --global http.sslVerify "false"
```

