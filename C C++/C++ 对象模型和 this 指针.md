---
title: C++ 对象模型和 this 指针
aliases: 
author: SprInec
date: 2024-12-07
update: 2024-12-07 18:18:05
reference:
  - 黑马程序员C++教程
tags:
  - cpp
---
# C++ 对象模型和 this 指针

### 1. 成员变量和成员函数分开存储

在 C++ 中，类内的成员变量和成员函数分开存储，<u>只有非静态成员变量才属于类的对象上</u>

```cpp
class Person
{
	int m_a;             // 非静态成员变量，属于类的对象上
	static m_b;          // 静态成员变量，不属于类的对象上
	void func1();        // 非静态成员函数，不属于类的对象上
	static void func2(); // 静态成员函数，不属于类的对象上
};
```

>[!Note] 
> 
> **空对象占用内存空间为: 1**
> 
> C++ 编译器会给每个空对象也分配一个字节空间，是为了区分空对象所占内存的位置，每个空对象也应有独一无二的内存地址

### 2. this 指针概念

由 [3.1](#3.1%20成员变量和成员函数分开存储) 我们得知在C++中成员变量和成员函数是分开存储的，每一个非静态成员函数会生成一份函数实例，也就是说多个同类型的对象会共用一块代码。那么问题是：<u>这一块代码是如何区分是哪个对象调用自己的呢？</u>

C++通过提供特殊的对象指针，`this` 指针，解决上述问题，**`this` 指针指向被调用的成员函数所属的对象**

`this` 指针是隐含每一个非静态成员函数内的一种指针

`this` 指针不需要定义，直接使用即可

>[!Note] 
> 
> `this` 指针的本质是**指针常量**，指针的指向是不可以修改的。

`this` 指针的用途：

- 当形参和成员变量同名时，可用 `this` 指针来区分
- 在类的非静态成员函数中返回对象本身，可使用 `return *this`

```cpp hl:5,10,20
class Person
{
	Person(int age)
	{
		this->age = age;
	}
	Person& addPerson(Person p)
	{
		this->age = p.age;
		return *this;
	}
};

void call_func()
{
	Person p1(10);
	Person p2(10);

	/* 链式编程思想 */
	p2.addPerson(p1).addPerson(p1).addPerson(p1);
}
```

### 3. 空指针访问成员函数

C++中空指针也是可以调用成员函数的，但是也要注意有没有用到 `this`  指针

如果用到 `this` 指针，需要加以判断保证代码的健壮性


```cpp error:23 info:12
class Person
{
public:

	void showClassName()
	{
		cout << "this is person class" << endl;
	}

	void showPersonAge()
	{
		cout << "age: " << m_age << endl;
	}

	int m_age;
};

void call_func()
{
	Person *p = NULL;

	p->showClassName();
	p->showPersonAge(); // ;LiAlertTriangle; ERROR: 空指针访问错误
}
```

修改如下，增强代码健壮性：

```cpp hl:12-15
class Person
{
public:

	void showClassName()
	{
		cout << "this is person class" << endl;
	}

	void showPersonAge()
	{
		if (this == NULL)
		{
			return;
		}
		cout << "age: " << m_age << endl;
	}

	int m_age;
};

void call_func()
{
	Person *p = NULL;

	p->showClassName();
	p->showPersonAge(); // ;LiAlertTriangle; ERROR: 空指针访问错误
}
```

### 4. const 修饰成员函数

**常函数：**

- 成员函数<u>后</u>加 `const` 后称为**常函数** 
- 常函数内不可以修改成员属性
- 成员属性声明时加关键字 `mutable` 后，在常函数中依然可以修改

**常对象：**
- 声明对象前加 `const` 称该对象为**常对象**
- 常对象只能调用常函数

```cpp hl:5,8,17,25,27 error:7,24,28
class Person
{
public:

	void showPerson() const
	{
		this->m_a = 10; // ;LiAlertTriangle; ERROR: 不可修改
		this->m_b = 10; // ;LiLightbulb; 可修改
	}

	void noconst_func()
	{
		m_a = 100;
	}

	int m_a;
	mutable int m_b;
};

void call_func()
{
	const Person p; 

	p.m_a = 100; // ;LiAlertTriangle; ERROR: 不可修改
	p.m_b = 100; // ;LiLightbulb; 可修改

	p.showPerson();
	p.noconst_func(); // ;LiAlertTriangle; ERROR: 常对象不可以调用普通成员函数, 因为普通成员函数可以修改成员属性
}
```
