---
title: string 容器
aliases: 
author: SprInec
date: 2024-12-14
update: 2024-12-15 19:27:42
reference:
  - 黑马程序员C++教程
tags: cpp, stl, 泛型编程
---
# string 容器

### 1. string 基本概念

**本质**：`string` 是 C++ 风格的字符串，而 `string` 本质上是一个类

**`string` 和 `char *` 的区别**：
- `char *` 是一个指针
- `string` 是一个类，内部封装了 `char *`，管理这个字符串，是一个 `char *` 型的容器。

特点：
- `string` 类内部封装了很多成员方法如：
	- 查找 `find`、拷贝 `copy`、删除 `delete`、替换 `replace`、插入 `insert`
- `string` 管理 `char *` 所分配的内存，不用担心复制越界和取值越界等问题，由类内部负责

### 2. string 构造函数

构造函数原型:
- `{cpp icon}string()`                               - 创建一个空的字符串，如 `string str`;
- `{cpp icon}string(const char *s)`        - 使用字符串 `s` 初始化
- `{cpp icon}string(const string &str)` - 使用一个 `string` 对象初始化另一个 `string` 对象
- `{cpp icon}string(int n, char c)`        - 使用 `n` 个字符 `c` 初始化

```cpp hl:7,9,10,14,17 fix:2
#include <iostream>
#include <string>
using namespace std;

void test_func()
{
	string s1;

	const char *str = "hello world";
	string s2(str);

	cout << "s2= " << s2 << endl; // output: hello world

	string  s3(s2);
	cout << "s3= " << s3 << endl; // output: hello world

	string s4(10, 'a');
	cout << "s4= " << s4 << endl; // output: aaaaaaaaaa
}
```

>[!Summary] 
> 
> `string` 的多种构造方式没有可比性，灵活使用即可

### 3. string 赋值操作

赋值的函数原型：
- `{cpp icon}string& operator=(const char *s);`        - `char *` 类型字符串 `s` 赋值给当前字符串
- `{cpp icon}string& operator=(const string &s);`     - 把字符串 `s` 赋值给当前字符串
- `{cpp icon}string& operator=(char c);`                    - 把字符 `c` 赋值给当前字符串
- `{cpp icon}string& assign(const char *s);`             - 把字符串 `s` 赋值给当前字符串
- `{cpp icon}string& assign(const char *s, int n);` - 把字符串 `s` 的前 `n` 个字符赋值给当前字符串
- `{cpp icon}string& assign(const string &s);`         - 把字符串 `s` 赋值给当前字符串
- `{cpp icon}string& assign(int n, char c);`             - 把 `n` 个字符 `c` 赋值给当前字符串

```cpp hl:8,11,14,17,20,23,26
#include <iostream>
#include <string>
using namespace std;

void test_func()
{
	string str1;
	str1 = "hello world";

	string str2;
	str2 = str1;

	string str3;
	str3 = 'a';

	string str4;
	str4.assign("hello world");

	string str5;
	str5.assign("hello world", 5);

	string str6;
	str6.assign(str5);

	string str7;
	str6.assign(10, 'a');
}
```

>[!Summary] 
> 
> `string` 的赋值方式有很多，`operator=` 的这种方式更为实用
### 4. string 字符串拼接

函数原型：
- `{cpp icon}string& operator+=(const char *str);`                      - 重载 `+=` 操作符
- `{cpp icon}string& operator+=(const char c );`                         - 重载 `+=` 操作符
- `{cpp icon}string& operator+=(const string &str);`                  - 重载 `+=` 操作符
- `{cpp icon}string& append(const char *s);`                                - 把字符串 `s` 连接到当前字符串结尾
- `{cpp icon}string& append(const char *s, int n);`                    - 把字符串 `s` 的前 `n` 个字符串连接到当前字符串结尾
- `{cpp icon}string& append(const  string &s);`                           - 同 `{cpp icon}operator+=(const string &str);`
- `{cpp icon}string& append(const string &s, int pos, int n);` - 字符串 `s` 中从 `pos` 开始的 `n` 个字符串连接到当前字符串结尾

```cpp hl:9,11,13-14,16,18,20,22,24
#include <iostream>
#include <string>
using namespace std;

void test_func()
{
	string str1 = "我";

	str1 += "爱玩游戏";

	str1 += ':';

	string str2 = "黑神话悟空"
	str1 += str2;

	string str3 = "I";

	str3.append("love");

	str3.append(game abc", 4)

	str3.append(str2);

	str3.append(str2, 3, 2);
}
```

### 5. string 查找和替换

查找：查找指定字符串是否存在
替换：在指定的位置替换字符串

函数原型：
- `{cpp icon}int find(const string &str, int pos = 0) const;`        - 查找 `str` 第一次出现的位置，从 `pos` 开始查找
- `{cpp icon}int find(const char *s, int pos = 0) const;`               - 查找 `s` 第一次出现的位置，从 `pos` 开始查找
- `{cpp icon}int find(const char *s, int pos, int n) const;`          - 从 `pos` 位置查找 `s` 的前 `n` 个字符第一次出现的位置
- `{cpp icon}int find(const char c, int pos = 0) const;`                 - 查找字符 `c` 第一次出现位置
- `{cpp icon}int rfind(const string &str, int pos = npos) const;` - 查找 `str` 最后一次出现位置，从 `pos` 开始查找
- `{cpp icon}int rfind(const char *s, int pos = npos) const;`        - 查找 `s` 最后一次出现的位置，从 `pos` 开始查找
- `{cpp icon}int rfind(const char *s, int pos, int n) const;`        - 从 `pos` 位置查找 `s` 的前 `n` 个字符最后一次出现的位置
- `{cpp icon}int rfind(const char c, int pos = 0) const;`               - 查找字符 `c` 最后一次出现位置
- `{cpp icon}string& replace(int pos, int n, const string& str);` - 替换从 `pos` 开始的 `n` 个字符为字符串 `str`
- `{cpp icon}string& replace(int pos, int n, const char *s);`        - 替换从 `pos` 开始的 `n` 个字符为字符串 `s`

> [!Quote]
> 
> ![C++ 对象模型和 this 指针](C++%20对象模型和%20this%20指针.md#4%20const%20修饰成员函数)

```cpp hl:9,10,12
#include <iostream>
#include <string>
using namespace std;

void test_func()
{
	string str = "abcdefde";

	int pos = str.find("de"); // output: 3
	pos = str.find("df"); // output: -1

	pos = str.rfind("de"); // output: 6
}
```

>[!Note] 
> 
> - `find` 从左往右 ➡ 找，`rfind` 从右往左 ⬅ 找
> - `find`/`rfind` 找到字符后返回查找到的字符的位置，找不到返回 `-1`

```cpp hl:10
#include <iostream>
#include <string>
using namespace std;

void test_func()
{
	string str = "abcdefg";

	/* 从 1 号字符起，三个字符替换为 1111 */
	str.replace(1, 3, "1111"); // result: str = a1111efg
}
```

>[!Note] 
> 
> `replace` 在替换时，需要指定从那个位置起，多少字符，替换成什么样的字符串

### 6. string 字符串比较

比较方式：字符串比较是按照字符的 ASCII 码进行比较

- `=` 返回 0
- `>` 返回 1
- `<` 返回 -1

函数原型：
- `{cpp icon}int compare(const string &s) const;` - 与字符串 `s` 比较
- `{cpp icon}int compare(const char *s) const;`     - 与字符串 `s` 比较

```cpp hl:10,14
#include <iostream>
#include <string>
using namespace std;

void test_func()
{
	string str1 = "abcdefg";
	string str2 = "abcdefg";

	if (str1.compare(str2) == 0)
	{
		cout << "equal" << endl;
	}
	else if ((str1.compare(str2) > 0)
	{
		cout << "str1 > str2" << endl;
	}
	else
	{
		cout << "str1 < str2" << endl;
	}
}
```

>[!Summary] 
> 
> 字符串对比主要是用于比较两个字符串是否相等，判断大小的意义并不是很大

### 7. string 字符存取

`string` 字单个字符存取方式有两种
- `{cpp icon}char& operator[](int n);` - 通过 `[]` 方式取字符
- `{cpp icon}char& at(int n);`               - 通过 `at` 方式获取字符

```cpp hl:12,17,21,22 fix:10
#include <iostream>
#include <string>
using namespace std;

void test_func()
{
	string str = "hello";

	/* 获取字符 */
	for （int i = 0; i < str.size(); i++)
	{
		cout << str[i] << endl;
	}

	for （int i = 0; i < str.size(); i++)
	{
		cout << str.at(i) << endl;
	}

	/* 修改字符 */
	str[0] = 'x';
	str.at(1) = 'x';
}
```

### 8. string 的插入和删除

对 `string` 字符串进行插入和删除字符操作

函数原型：
- `{cpp icon}string& insert(int pos, const char *s);`        - 插入字符串
- `{cpp icon}string& insert(int pos, const string &str);` - 插入字符串
- `{cpp icon}string& insert(int pos, int n, char c);`        - 在指定位置插入 `n` 个字符 `c`
- `{cpp icon}string& erase(int pos, int n = npos);`            - 删除从 `pos` 开始的 `n` 个字符串

```cpp hl:9,12
#include <iostream>
#include <string>
using namespace std;

void test_func()
{
	string str = "hello";

	str.insert(1, "xx");
	cout << str << endl; // output: hxxello

	str.erase(1, 3);
	cout << str << endl; // output: hello
}
```

>[!Note] 
> 
> 插入和删除的起始下标都是从 0 开始

### 9. string 字串

从字符串中获取想要的子串

函数原型：
- `{cpp icon}string substr(int pos = 0, int n = npos) const;` - 返回由 `pos` 开始的 `n` 个字符组成的字符串

```cpp hl:9,18,20
#include <iostream>
#include <string>
using namespace std;

void test1_func()
{
	string str = "hello";

	string sub_str = str.substr(1, 3);
	cout << sub_str << endl; // output: ell	
}

/* 实用操作 */
void test2_func()
{
	string email = "testemail@example.com"

	int pos = email.find("@");

	string user_name = email.substr(0, pos);
	cout << user_name << endl; // output: testemail
}
```

>[!Summary] 
> 
> 灵活的运用 `substr` 功能，可以在实际开发中获取有效信息

