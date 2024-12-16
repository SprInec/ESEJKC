---
title: vector 容器
aliases: 
author: SprInec
date: 2024-12-15
update: 2024-12-16 16:11:56
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

### 2. vector 构造函数

创建 `vector` 容器

函数原型：
- `{cpp icon}vector<T> v;` - 采用模板类实现，默认构造函数
- `{cpp icon}vector(v.begin(), v.end()); ` - 将 `v[ begin(), end() )` 区间中的元素拷贝给本身
- `{cpp icon}vector(n, elem);` - 构造函数将 `n` 个 `elem` 拷贝给本身 
- `{cpp icon}vector(const vector &vec); ` - 拷贝构造函数 

```cpp hl:17,26,30,34
#include <iostream>
#include <vector>
using namespace std;

void printVector(vector<int> &v)
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

void test_func()
{
	/* 默认构造，无参构造 */
	vector<int> v1;

	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	/* 通过区间方式进行构造 */
	vector<int> v2(v1.begin(), v1.end());
	printVector(v2);

	/* n 个 elem 方式构造 */
	vector<int> v3(3, 100);
	printVector(v3); // output: 100 100 100

	/* 拷贝构造 */
	vector<int> v4(v3);
}
```

### 2. vector 赋值操作

给 `vector` 容器进行赋值

函数原型：
- `{cpp icon}vector& operator=(const vector &vec);` - 重载 `operator=` 操作符
- `{cpp icon}assign(beg, end);` - 将 `[beg, end)` 区间中的数据拷贝赋值给本身
- `{cpp icon}assign(n, elem);` - 将 `n` 个 `elem` 拷贝赋值给本身

```cpp hl:25,28,32
#include <iostream>
#include <vector>
using namespace std;

void printVector(vector<int> &v)
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

void test_func()
{
	vector<int> v1;

	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int> v2;
	v2 = v1;
	printVector(v2);

	vector<int> v3;
	v3.assign(v1.begin(), v1.end());
	printVector(v3);

	vector<int> v4;
	v4.assign(3, 100);
	printVector(v4);
}
```

### 3. vector 容量和大小

对 `vector` 容器的容量和大小操作

函数原型：
- `{cpp icon}empty();` - 判断容量是否为空
- `{cpp icon}capacity();` - 容器的容量
- `{cpp icon}size();` - 返回容器中元素的个数
- `{cpp icon}resize(int num);` - 重新指定容器的长度为 `num` ，若容器变长，则以默认值 `0` 填充新位置，如果容器变短，则默认超出容器长度的元素被删除
- `{cpp icon}resize(int num, elem);` - 重新指定容器的长度为 `num` ，若容器变长，则以 `elem` 填充新位置，如果容器变短，则默认超出容器长度的元素被删除

```cpp hl:24,31-32,35-37
#include <iostream>
#include <vector>
using namespace std;

void printVector(vector<int> &v)
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

void test_func()
{
	vector<int> v1;

	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	if (v1.empty())
	{
		cout << "empty" << endl;
	}
	else
	{
		cout << "not empty" << endl;
		cout << "capacity: " << v1.capacity() << endl;
		cout << "size: " << v1.size() << endl;
	}

	v1.resize(15);
	v1.resize(17, 100);
	v1.resize(5);
}
```

### 4. vector 插入和删除

对 `vector` 容器进行插入和删除

函数原型：
- `{cpp icon}push_back(ele);` - 尾部插入元素 `ele`
- `{cpp icon}pop_back();` - 删除最后一个元素
- `{cpp icon}insert(const_iterator pos, ele);` - 迭代器指向位置 `pos` 中插入元素 `ele`
- `{cpp icon}insert(const_iterator pos, int count, ele)` - 迭代器指向位置 `pos` 中插入 `count` 个元素 `ele`
- `{cpp icon}erase(const_iterator pos);` - 删除迭代器指定的元素
- `{cpp icon}erase(const_iterator start, const_iterator end);` - 删除迭代器从 `start` 到 `end` 之间的元素
- `{cpp icon}clear();` - 删除容器中所有元素

```cpp hl:21,26,30,33,37,41-42
#include <iostream>
#include <vector>
using namespace std;

void printVector(vector<int> &v)
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

void test_func()
{
	vector<int> v1;

	for (int i = 0; i < 10; i++)
	{
		/* 尾插 */
		v1.push_back(i);
	}
	printVector(v1);

	/* 尾删 */
	v1.pop_back();
	printVector(v1);

	/* 插入，第一个参数为迭代器 */
	v1.insert(v1.begin(), 100);
	printVector(v1);

	v1.insert(v1.begin(), 2, 200);
	printVector(v1);

	/* 删除 */
	v1.erase(v1.begin());
	printVector(v1);

	/* 清空 */
	v1.erase(v1.begin(), v1.end());
	v1.clear();
	printVector(v1);
}
```

### 5. vector 数据存取

对 `vector` 中的数据进行存取操作

函数原型：
- `{cpp icon}at(int idx);` - 返回索引 `idx` 所指的数据
- `{cpp icon}operator[];` - 返回索引 `idx` 所指的数据
- `{cpp icon}front();` - 返回容器中第一个数据元素
- `{cpp icon}back();` - 返回容器中最后一个数据

```cpp hl:26,33,38,41
#include <iostream>
#include <vector>
using namespace std;

void printVector(vector<int> &v)
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

void test_func()
{
	vector<int> v1;

	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}

	/* 利用 [] 方式存取数据元素 */
	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1[i] << " ";
	}
	cout << endl;

	/* 利用 at 方式访问元素 */
	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1.at(i) << " ";
	}
	cout << endl;

	/* 获取第一个元素 */
	cout << v1.front() << endl;

	/* 获取最后一个元素 */
	cout << v1.back() << endl;
}
```

>[!Summary] 
> 
> - 除了用迭代器获取 `vector` 容器中的元素，`[]` 和 `at()` 也可以获取
> - `front` 返回容器第一个元素
> - `back` 返回容器中的最后一个元素

### 6. vector 互换容器

实现两个容器内元素进行互换

函数原型：
- `{cpp icon}swap(vec);` - 将 `vec` 与本身的元素互换

```cpp hl:32,54
#include <iostream>
#include <vector>
using namespace std;

void printVector(vector<int> &v)
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

/* 1. 基本使用 */
void test1_func()
{
	vector<int> v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	cout << "befor swap: " << endl;
	printVector(v1);

	vector<int> v2;
	for (int i = 10; i > 0; i--)
	{
		v2.push_back(i);
	}
	printVector(v2);

	cout << "after swap: " << endl;
	v1.swap(v2);
	printVector(v1);
	printVector(v2);
}

/* 2. 实际用途：巧用 swap 可以收缩内存空间 */
void test2_func()
{
	vector<int> v;
	for (int i = 0; i < 10000; i++)
	{
		v1.push_back(i);
	}

	cout << "capacity: " << v.capacity() << endl;
	cout << "size: " << v.size() << endl;

	v.resize(3); // 重新指定大小
	cout << "capacity: " << v.capacity() << endl;
	cout << "size: " << v.size() << endl;

	/* 匿名对象结合 swap 收缩内存 */
	vector<int>(v).swap(v);

	cout << "capacity: " << v.capacity() << endl;
	cout << "size: " << v.size() << endl;
}
```

>[!Summary] 
> 
> `swap` 可以使两个容器互换，从而达到收缩内存的效果

### 7. vector 预留空间

减少 `vector` 在动态扩展容量时的扩展次数

函数原型：
- `{cpp icon}reserve(int len);` - 容器预留 `len` 个元素长度，预留位置不初化，元素不可访问

```cpp hl:10
#include <iostream>
#include <vector>
using namespace std;

void test_func()
{
	vector<int> v;

	/* 利用 reserve 预留空间 */
	v.reserve(10000);

	/* 统计空间开辟次数 */
	int num = 0;
	int *p = NULL;
	for (int i = 0; i < 10000; i++)
	{
		v1.push_back(i);

		if (p != &v[0])
		{
			p = &v[0];
			num++;
		}
	}
	cout << num <<endl;
}
```

>[!Summary] 
> 
> 如果数据量较大，可以一开始利用 `reserve` 预留空间

