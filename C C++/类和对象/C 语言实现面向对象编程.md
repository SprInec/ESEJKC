---
title: C语言实现面向对象编程
aliases: 
author: SprInec
date: 2024-12-16
update: 2024-12-16 21:50:06
reference: []
tags: c
---
# C 语言实现面向对象编程

在 C 语言中，没有像 C++ 中的类和成员函数的直接支持，但可以通过结构体和[函数指针](指针.md#5%202%20函数指针（Function%20Pointer）)的组合来模拟 C++ 中的类和成员函数的行为。这是一种面向对象编程的模拟方法。
### C++ 类的功能模拟

C++ 类的主要特性包括：

1. **数据封装**：类的数据和函数通常是封装在一起的。
2. **成员函数**：类的成员函数能够操作类的数据。
3. **继承与多态**：通过虚函数和继承实现。

在 C 语言中，可以通过结构体存储数据，并使用函数指针模拟成员函数的行为，从而模拟类的功能。

### 1. 使用结构体和函数指针模拟类

假设需要模拟一个简单的 `Car` 类，包含属性（如 `color`）和一个成员函数（如 `display`）。

首先定义一个 `Car` 结构体，结构体内包含数据成员（如 `color`）和一个指向成员函数的函数指针（如 `display`）。

```c
#include <stdio.h>
#include <string.h>

// 定义一个结构体模拟类
typedef struct {
    char color[20];
    // 函数指针，模拟类中的成员函数
    void (*display)(void*); // display 函数接受一个指向结构体的指针
} Car;

// display 函数的实现，模拟成员函数
void displayCar(void* obj) {
    Car* car = (Car*)obj;
    printf("This car is %s.\n", car->color);
}

// 创建并初始化 Car 实例的函数
void initCar(Car* car, const char* color) {
    strcpy(car->color, color);
    car->display = displayCar;  // 将函数指针指向 displayCar 函数
}

int main() {
    Car myCar;
    initCar(&myCar, "red");

    // 使用结构体和函数指针调用成员函数
    myCar.display(&myCar); // 通过函数指针调用 display 函数

    return 0;
}
```

1. **结构体模拟数据成员**：
    - `Car` 结构体中的 `color` 字段是模拟类的成员变量。

2. **函数指针模拟成员函数**：
    - `display` 是一个函数指针，指向一个接受 `Car*` 参数的函数。它模拟了 C++ 类中的成员函数。在这个例子中，`displayCar` 函数就相当于 C++ 类中的一个成员函数，负责显示 `Car` 对象的 `color`。

3. **初始化结构体实例**：
    - `initCar` 函数模拟了 C++ 类的构造函数，用于初始化结构体数据和设置函数指针。

4. **调用成员函数**：
    - 使用 `myCar.display(&myCar)` 来调用函数指针指向的函数，这类似于在 C++ 中通过 `object.display()` 调用成员函数。

### 2. 模拟更多成员函数

假设我们要在 `Car` 类中添加一个修改颜色的成员函数 `setColor`，可以通过函数指针来实现。

```c
#include <stdio.h>
#include <string.h>

// 定义结构体模拟类
typedef struct {
    char color[20];
    void (*display)(void*);  // 显示颜色
    void (*setColor)(void*, const char*);  // 设置颜色
} Car;

// display 函数的实现
void displayCar(void* obj) {
    Car* car = (Car*)obj;
    printf("This car is %s.\n", car->color);
}

// setColor 函数的实现
void setCarColor(void* obj, const char* newColor) {
    Car* car = (Car*)obj;
    strcpy(car->color, newColor);
    printf("The car color has been updated to %s.\n", car->color);
}

// 初始化 Car 实例
void initCar(Car* car, const char* color) {
    strcpy(car->color, color);
    car->display = displayCar;  // 设置 display 函数指针
    car->setColor = setCarColor;  // 设置 setColor 函数指针
}

int main() {
    Car myCar;
    initCar(&myCar, "red");

    // 使用 display 函数指针调用成员函数
    myCar.display(&myCar);

    // 使用 setColor 函数指针修改成员变量
    myCar.setColor(&myCar, "blue");
    myCar.display(&myCar);

    return 0;
}
```

1. **函数指针扩展**：新增了 `setColor` 函数指针，模拟了 C++ 类中的成员函数 `setColor`，用于修改 `Car` 对象的 `color`。
2. **函数调用**：通过 `myCar.setColor(&myCar, "blue")` 调用 `setColor` 函数指针，修改 `Car` 对象的颜色。

### 3. 模拟继承

C 语言没有直接的继承机制，但可以通过**嵌套结构体**和**函数指针**的方式来模拟。这里可以模拟一个 `SportsCar` 继承自 `Car` 的例子。

```c
#include <stdio.h>
#include <string.h>

// 基类 Car
typedef struct {
    char color[20];
    void (*display)(void*);  
    void (*setColor)(void*, const char*);
} Car;

void displayCar(void* obj) {
    Car* car = (Car*)obj;
    printf("This car is %s.\n", car->color);
}

void setCarColor(void* obj, const char* newColor) {
    Car* car = (Car*)obj;
    strcpy(car->color, newColor);
    printf("The car color has been updated to %s.\n", car->color);
}

void initCar(Car* car, const char* color) {
    strcpy(car->color, color);
    car->display = displayCar;
    car->setColor = setCarColor;
}

// 派生类 SportsCar
typedef struct {
    Car base;  // 继承自 Car
    int topSpeed;
    void (*displaySpeed)(void*);
} SportsCar;

void displaySpeed(void* obj) {
    SportsCar* sportsCar = (SportsCar*)obj;
    printf("This sports car has a top speed of %d km/h.\n", sportsCar->topSpeed);
}

void initSportsCar(SportsCar* car, const char* color, int speed) {
    initCar(&car->base, color);  // 初始化基类部分
    car->topSpeed = speed;
    car->displaySpeed = displaySpeed;
}

int main() {
    SportsCar mySportsCar;
    initSportsCar(&mySportsCar, "red", 300);

    // 使用继承自 Car 的 display 函数
    mySportsCar.base.display(&mySportsCar);

    // 使用 SportsCar 的特有 displaySpeed 函数
    mySportsCar.displaySpeed(&mySportsCar);

    return 0;
}
```

1. **模拟继承**：`SportsCar` 结构体通过嵌套 `Car` 结构体来模拟继承。这意味着 `SportsCar` 拥有 `Car` 的所有数据和函数指针。
2. **函数调用**：在 `main` 函数中，`mySportsCar.base.display()` 调用了继承自 `Car` 的 `display` 函数，而 `mySportsCar.displaySpeed()` 调用了 `SportsCar` 特有的 `displaySpeed` 函数。

>[!Summary] 
>
>通过结构体和函数指针，C 语言能够模拟 C++ 类的核心特性：
>
> 1. **封装**：通过结构体存储数据。
> 2. **成员函数**：通过函数指针模拟成员函数。
> 3. **继承**：通过嵌套结构体和共享函数指针模拟继承。
> 
> 尽管 C 语言缺乏 C++ 中的真正类和继承机制，但使用这种方法，可以实现面向对象编程的基本思想。

