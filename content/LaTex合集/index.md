+++
title = "LaTex合集"
date = 2022-03-29 15:36:14
slug = "202203291536"

[taxonomies]
tags = ["LaTex" ]
categories = ["LaTex"]

+++

<!-- more -->

## LaTex不能显示中文生僻字

没错，就是我的名字

事故现场：

```latex
\documentclass[
    latin-font = win|mac|linux|gyre|none,
    cjk-font = win|mac|linux|fandol|founder|noto|source|none,
    ]{njuthesis}
```

解决：把字体设置里的其他系统删掉（X

```latex
\documentclass[
    latin-font = win,
    cjk-font = win,
    ]{njuthesis}
```

