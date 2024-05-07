---
title: Effective C++
katex: true
mathjax: false
date: 2024-04-27 18:01:37
tags:
cover_image: /imgs/coverImgs/c++.jpg
---

>写这篇笔记的主要原因并不是为了记下所有的内容，而是更像是一个导读的作用，我会用自己的语言写下每个规则都在介绍什么，尝试解决什么样的问题，出现了什么新颖的解决办法/思路，为什么要这么做，如果不这么做会导致什么问题以及在书中碰到了什么不懂得概念。具体细节请返回原书，没有必要把书上有的代码再重复一遍。我觉得这是一个尽最大限度帮我整理整书的内容，并且在今后遇到类似问题的时候提供一个方法册。

# 序言随记

## 01. 显示的申明构造函数

**好处:** 

- 阻止无法预知的隐式转换

Things you should know:

- default构造函数，要不是没有参数，要么就是参数都有缺省值，比如

 ```c++
 class A {
 public:
     A();          // default
     A(int x = 0); // default
     A(int x);     // non-default
 }
 ```

 **PS: 更多有关转型的tips请看本书条款27**

## 02. copy constructor and copy assignment

注意区分他们的区别，copy构造函数是以同类型的对象初始化自我对象，而copy赋值函数是从另一个同类型对象中拷贝其值到自我对象中。**一般来说如果你没有为你的类定义这两个函数，那么编译器就会自动帮你生成这两个函数在需要的时候**。

一般他们在类中的定义如下：

```c++
class A {
public:
    ...;
    A(const A& rhs);            // copy constructor
    A& operator=(const A& rhs); // copy assignment
}
```

**区别何时调用的这两个函数：**只需要看是否有新的对象被创建

```c++
A testOne;
A testTwo(testOne);    // cpoy constructor
A testThree = testTwo; // copy constructor
testOne = testThree;   // copy assignment
```

## TR1和Boost

这两个其实就是对stl库的扩展，tr1增加很多c++11之后的功能，而Boost是一个类似于大杂烩的库，针对现有stl库的不足，Boost社区的程序员自发的把自己实现的好用的库开源出来给大家使用，现在的c++库也在不断的完善，所有在学习这些比较老旧的技术之前先看看最新的比如c++17，c++20等等。

## 条款1：视c++为语言联邦而不是单一的语言

由于c++的特性较多，因此**c++高效编程准则**有可能会因为不同的特性而变化，本书将c++划分成了更加细致的4个次语言：**C, Object-Oriented C++, Template C++ and STL**。而不同的次语言所遵循的高效编程准则都不太一样，就以传递参数为例：
C和STL在传递参数的时候，pass-by-value更高效，相反对于Object-Oriented C++和Template C++来说pass-by-reference-to-const则更加高效。


## 条款2：尽量使用const，enum，inline替换#define

**使用#define的坏处**：
- 常量定义: 可能导致定义的常量没有进入符号表，因为该常量在进入编译器之前就被预编译器移走了。**可能会引起提到该常量值的编译错误**。
- 宏函数: 

**使用const定义常量的好处**：
- 解决常量不会被编译器看到

**使用const需要注意的特殊情况**:
- 常量指针: 由于常量一般被定义在头文件中供多个源文件使用，当多个源文件创建指向这个常量的指针的时候会引起不确定性，所以定义常量指针就显得很有必要
- class专属常量: 为了确保多个实体所拥有的常量都是一致的，申明该常量为static很有必要
- 如果**不取地址**那么声明整数类变量（c++ integral type）可以无须提供定义式，一些旧的编译器不允许static成员**在其声明式**上获得初值，那么初值可以放在定义式中。
- 编译期间若需要一个class的常量值，比如**数组的大小**，但你的编译器不允许完成"in class"初值设定，可以使用**Enum hack**的方法

```c++
class GamePlayer {
private:
    enum { NumTurns = 5 };

    int scores[NumTurns];
    // ... 
}
```

### 一些新的概念

1. 预处理器实际就是一些指令，这些指令告诉编译器在编译之前应该完成哪些动作，常见的预处理指令有：
- #define预处理

```c++
// 常量
#define PI 3.14
// 宏函数
#define MIN(a, b) (((a)<(b)) ? a : b)
```
- #include
- 条件编译
```c++
#ifndef NULL
    #define NULL 0
#endif

#if 0
    ...
#endif
```

2. 符号表

3. the enum hack是**模版源编程**的基本技术之一，其好处有：
- 限制获取整数常量的pointer或reference
- 防止某些编译器不遵循**对整数型const对象不另设存储空间**的约定而导致的不必要的内存分配

4. macro, function and macro function的区别

5. template inline函数，见条款30

## 条款3：尽量使用const

## 条款4：确定对象在被使用前已被初始化
