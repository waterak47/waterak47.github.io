---
title: Java基础：I/O操作
date: 2017-11-09 10:39:41
comments: true
tags: Java基础
categories: Java
---
### I/O简介
I/O就是输入和输出，核心是I/O流，流用于读写设备上的数据，包括硬盘文件、内存、键盘、网络...

> 根据数据的走向分为：输入流、输出流
> 根据处理的数据类型分为：字节流、字符流

字节流能处理所有类型数据，对应的类以`Stream`结尾。
字符流只能处理文本数据，对应的类以`Reader`或`Writer`结尾。

<!--more-->

各种流的继承关系:

![](https://ws1.sinaimg.cn/large/006tKfTcgy1flbn1ooov2j30k40laq56.jpg)