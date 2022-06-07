+++
title = "LaTex使用总结"
date = 2022-01-29 15:36:14
slug = "202201291536"

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

#### 单段不首行缩进

```latex
\noindent{
	不需要首行缩进的段落内容
}
```



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

## 插入表格

[TablesGenerator](https://www.tablesgenerator.com/latex_tables)

### 定义表格单列宽度

```
\begin{tabular}{p{2cm}p{3cm}p{2.5cm}p{2.5cm}p{2.5cm}p{2.5cm}}
```

`p{2cm}`就是代表第一列的宽度，后面依次是后面的宽度，会自动进行换行显示。

### 按页面宽度调整表格

`\setlength{\tabcolsep}{7mm}{XXXX}`

```latex
\begin{center}
\textbf{Table 2}~~Improved table.\\
\setlength{\tabcolsep}{7mm}{
\begin{tabular}{cccccc} \toprule
Models  &  $\hat c$  &  $\hat\alpha$  &  $\hat\beta_0$  &  $\hat\beta_1$  &  $\hat\beta_2$  \\ \hline
model  & 30.6302  & 0.4127  & 9.4257  & - & - \\
model  & 12.4089  & 0.5169  & 18.6986  & -6.6157  & - \\
model  & 14.8586  & 0.4991  & 19.5421  & -7.0717  & 0.2183  \\
model  & 3.06302  & 0.41266  & 0.11725  & - & - \\
model  & 1.24089  & 0.51691  & 0.83605  & -0.66157  & - \\
model  & 1.48586  & 0.49906  & 0.95609  & -0.70717  & 0.02183  \\
\bottomrule
\end{tabular}}
\end{center}
```



### 表名

```latex
\begin{table}
\begin{tabular}{|l|l|l|l|}
	Any & 112  & 123 & 132  \\
	Zhang                   & 324& 345 & 345  \\
	David                   & 123                     & 34  & 45   \\
	Araminta                & 133 & 33  & 35 \\ 
\end{tabular}
\caption{a table of scores}
\end{table}
```
`\caption{a table of scores}`设置表名，如果不需要自动编号可以使用`\caption*{a table of scores}`



### 三行线表

```latex
	\toprule   %第一行线
    %表示第一列占1.5cm 第二列占6cm 第三列占2cm 的距离 并且这几个字都是居中对齐
    \multicolumn{1}{m{1.5cm}}{\centering Symbol} & \multicolumn{1}{m{6cm}}{\centering Definition} & \multicolumn{1}{m{2cm}}{ Unit} \\
    \midrule   %第二行线
     $V$ & index & -- \\
     $X$ & The  & -- \\
     $Y$ & The & -- \\
     $Z$ & The  & -- \\
    \bottomrule   %第三行线
```

### 全线表

```latex
\begin{table}[H]
\setlength{\tabcolsep}{20mm}{
\begin{tabular}{|l|l|}
\hline
第一行 & 的内容     \\ 
\hline
第二行 & 的内容     \\ 
\hline
\end{tabular}}
\caption {表名}
\end{table}
```



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

## 图片位置固定

为了避免图片位置被重排导致的图文分离，在`\begin{document}`前加上`\usepackage{float}`

由此在画图表的时候产生了另一个问题：

`You have forgotten to include a float specifier, which tells LaTeX where to position your figure. To fix this, either insert a float specifier inside the square brackets (e.g. \begin{figure}[h]), or remove the square brackets (e.g. \begin{figure}). Find out more about float specifiers here.`

解决办法：

位置固定用 `\begin{figure}[H]`

位置不固定用 `\begin{figure}[htp]`

## 不引用但是显示参考文献

（不要过问为什么不引用

```latex
\addcontentsline{toc}{section}{参考文献}
\nocite{1} %只加入到参考文献列表中，不在文中引用（显示[id]）
\nocite{*} %显示所有文献
\bibliographystyle{plain}
\begin{thebibliography}{}
\bibitem[1]{1} Book Title, Author
\end{thebibliography}

```

