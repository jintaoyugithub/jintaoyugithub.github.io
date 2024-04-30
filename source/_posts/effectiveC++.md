---
title: Effective C++
katex: true
mathjax: false
date: 2024-04-27 18:01:37
tags:
cover_image: /imgs/coverImgs/c++.jpg
---

# 序言随记

## 01. 显示的申明构造函数

**好处:** 

- 阻止无法预知的隐式转换

Things you should know:

- default构造函数，要不是没有参数，要么就是参数都有省值，比如

 ```c++
 class A {
 public:
     A();          // default
     A(int x = 0); // default
     A(int x);     // non-default
 }
 ```

 PS: 更多有关转型的tips请看本书条款27

## 02. copy constructor and copy assignment


## 条款1：视c++为语言联邦而不是单一的语言


## 条款2：尽量使用const，enum，inline替换 #define


## 条款3：尽量使用const

## 条款4：确定对象在被使用前已被初始化
