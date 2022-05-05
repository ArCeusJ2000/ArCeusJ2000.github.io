+++
title = "LaTex使用总结"
date = 2022-03-29 15:36:14
slug = "202203291536"

[taxonomies]
tags = ["LaTex" ]
categories = ["LaTex"]

+++

<!-- more -->

# 学习笔记

[Overleaf速查表](https://blog.csdn.net/weixin_39876450/article/details/112287980)

## 章节和分级标题

```latex
\chapter{章节}
\section{一级标题}
\subsection{二级标题}
```

## 换行

#### 创建一个新的段落

只需要在代码中添加一个空白行就可以了，下一个段落会有段首缩进

```latex
\documentclass{article}
\begin{document}
第一自然段，有首行缩进

第二自然段，有首行缩进
\end{document}

```

#### 不缩进换行

```latex
\documentclass{article}
\usepackage[utf8]{inputenc}

\begin{document}
这是一个自然段，有缩进\\
这是\\（两个反斜杠）产生的新行，没有缩进\newline
这是\newline产生的新行，没有缩进\hfill \break
这是\hfill \break产生的新行，没有缩进
\end{document}
```

- `\\`（两个反斜杠）

- `\newline`
- `\hfill \break`

以上三种换行方法效果相同

## 插入图片

### Overleaf 中导入图片

在文件夹中存放图片

```latex
\begin{figure}[htp]
    \centering
   	\includegraphics[width=0.8\textwidth,height=0.5\textwidth]{the.png}
    \caption{An image of a galaxy}
    \label{fig:galaxy}
\end{figure}

```

`\graphicspath{ {images/} }` 指令告诉 LaTeX  图片是被存储在 `Images/` 文件夹之中的，现在可以仅输入图片的文件名，不再需要输入它的路径了。

`\includegraphics[width=4cm]{InsertingImagesEx5}`需要注意的是，只需要输入图片的文件名，不包括它的后缀

# 问题解决

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

