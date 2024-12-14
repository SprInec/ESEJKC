---
title: vector 存放数据
aliases: 
author: SprInec
date: 2024-12-13
update: 2024-12-14 14:49:04
reference:
  - 黑马程序员C++教程
tags: cpp, 泛型编程
---
# vector 存放数据

`vector` 是 STL 中最常用的容器，可以理解为数组
### 1. vector 存放内置数据类型

- 容器：`vector`
- 算法：`for_each`
- 迭代器：`vector<int>::iterator`

```cpp 
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

void userPrint(int val)
{
	cout << val << endl;
}

void test_func()
{
	/* 创建一个 vector 容器 - 数组 */
	vector<int> v;

	/* 向容器中插入数据 */
	v.push_back(1);
	v.push_back(2);
	v.push_back(3);

	/* 通过迭代器访问容器的数据 */
	// 起始迭代器，指向容器中的第一个元素
	vector<int>::iterator itBegin = v.begin();
	// 结束迭代器，指向容器中的最后一个元素的下一个位置
	vector<int>::iterator itEnd = v.end();

	/* 第一种遍历方式 */
	while (itBegin != itEnd)
	{
		cout << *itBegin << endl;
		itBegin++;
	}

	/* 第二种遍历方式 */
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << endl;
	}

	/* 第三种遍历方式 - 利用 STL提供的遍历算法 */
	for_each(v.begin(), v.end(), userPrint);
}
```

### 2. vector 存放自定义数据类型

```cpp hl:24,38,41,48,62,64 fix:40
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
using namespace std;

class Person
{
public:
	Person(string name, int age)
	{
		this->m_name = name;
		this->m_age = age;
	}

public:
	string m_name;
	int m_age;
};

/* 存放自定义数据 */
void test1_func()
{
	vector<Person> v;

	Person p1("aa", 10);
	Person p2("bb", 20);
	Person p3("cc", 30);
	Person p4("dd", 40);
	Person p5("ee", 50);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);

	for (vector<Person>::iterator it = v.begin(); it != v.end(); it++)
	{
		/* *it. 或者 it-> 均可, it 本身是一个指针 */
		cout << "Name: " << *it.m_name << "Age: " << it->m_age << endl;
 	}
}

/* 存放自定义数据的指针 */
void test2_func()
{
	vector<Person *> v;

	Person p1("aa", 10);
	Person p1("bb", 20);
	Person p1("cc", 30);
	Person p1("dd", 40);
	Person p1("ee", 50);

	v.push_back(&p1);
	v.push_back(&p2);
	v.push_back(&p3);
	v.push_back(&p4);
	v.push_back(&p5);

	for (vector<Person *>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << "Name: " << *(*it).m_name << "Age: " << (*it)->m_age << endl;
 	}
}
```

### 3. vector 容器嵌套容器

```cpp hl:7,24,26
#include <iostream>
#include <vector>
using namespace std;

void test_func()
{
	vector<vector<int>> v;
	
	vector<int> v1;
	vector<int> v2;
	vector<int> v3;

	for (int i = 0; i < 3; i++)
	{
		v1.push_back(i + 1);
		v2.push_back(i + 2);
		v3.push_back(i + 3);
	}

	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);

	for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++)
	{
		for (vector<int>::iterator vit = (*it).begin; vit != (*it).end(); vit++)
		{
			cout << *vit << " ";
		}
		cout << endl;
	}
}
```

>[!Note] 
> 
> 关于 `*it` 的判断：`vector<>` 的 `<>` 括号中的类型即为 `*it` 的数据类型

