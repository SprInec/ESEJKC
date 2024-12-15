---
title: vector 容器
aliases: 
author: SprInec
date: 2024-12-15
update: 2024-12-15 20:24:17
reference:
  - 黑马程序员C++教程
tags: cpp, stl, 泛型编程
---
# vector 容器

### 1. vector 基本概念

`vector` 数据结构和数组非常相似，也称为**单端数组**

`vector` 与普通数组的区别：
- 数组是静态空间，`vector` 是可以**动态扩展**的

>[!Note] 
> 
> **动态扩展**：并不是在原有空间之后续接新空间，而是找更大的内存空间，然后将原数据拷贝到新空间，释放原空间

![[ESEJKC/C C++/STL/vector 容器图例.md#^group=CzJorJ2y|900|center]]

`vector` 容器的迭代器是支持随机访问的迭代器
