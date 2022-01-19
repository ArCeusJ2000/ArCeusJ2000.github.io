+++
title = "C++STL报错处理合集"
date = 2022-01-11 18:36:14
slug = "202201111836"

[taxonomies]
tags = ["C/C++" ]
categories = ["C/C++"]

+++

<!-- more -->

## 参数类型统一

### [Error] no matching function for call to 'min(int&, std::basic_string<char>::size_type)'

STL原型

```
template< class T > 
const T& min( const T& a, const T& b );
```

报错语句

```
min(n, str.size());
```

使用强制类型转换，改为

```
min(n, int(str.size()));
```

