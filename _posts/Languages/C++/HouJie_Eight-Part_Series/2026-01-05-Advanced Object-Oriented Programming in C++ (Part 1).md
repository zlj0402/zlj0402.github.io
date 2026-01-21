---
title: "C++面向对象高级编程(上)"
date: 2026-01-05 23:33:00 +0800
categories: [Languages, C++]
tag: [C++，C++面向对象高级编程(上)]
permalink: /posts/Languages/C++/八部曲
---

* TOC
{:toc}

---

# C++面向对象程序设计
英文：Object Oriented Programming, OOP
面向对象是一种观念，用这种观念来写程序，需要面向对象的语言，而C++就是一个支持面向对象观念的一种语言。

## 1. C++ 编程简介

### 应该具备的基础：
+ 曾经学过 procedural language（ C 语言最佳）
    - 变量（variables）
    - 类型（types）: int, float, char, struct...
    - 作用域（scope）: 像我们以前我们写一个函数，函数有大括号，大括号里头那变量（variables）在这里头的生命，这个就是它的作用域。
    - 循环（loops）: while, for
    - 流程控制：if-else, switch-case
+ 知道一个程序需要编译、链接才能被执行
    - 现在的软件开发工具很发达，所以有很多的集成环境。不管你过去是用一种比较比较低阶的用命令行的模式的形式去做，去编译链接。
    - 或者是在集成环境底下去建立一个程序，只要你曾经有过这样的经验就可以。
+ 知道如何编译和链接
    - （如何建立一个可运行程序）

### 我们的目标：
+ 培养正规的、大气的编程习惯
+ 以良好的形式编写 C++ class
    > 这里为什么谈到class呢？因为 C++ 代码里面的每一个单元，最重要的每一个单元就是一个class，所以这边就出现了这个字眼。
    {: .prompt-info }
    
    class 有两种经典的分类：
    - class without pointer members: `Complex`
    - class with pointer members: `String`

    > Object Based(基于对象)：单一 class 的设计
    {: .prompt-tip }

    学习 Classes 之间的关系：
    - 继承（inheritance）
    - 复合（composition）
    - 委托（delegation）

    > Object Oriented(面向对象)：几个class彼此之间有关联
    {: .prompt-tip }

    整个课程呢，是分为两大类，一个是基于对象，一个是面向对象。

    > 首先要培养好的对单一 class 的正规的，好的习惯写法。然后我们才可以扩大好几个 class 之间，好几个类之间，他们彼此有怎么样的关系
    {: .prompt-info }

### 将习得的代码
  + Object Based:
    - without pointer
  
      complex.h<br>
      complex-test.cpp
    - with pointer
  
      string.h<br>
      string-test.cpp
  + Object Oriented:
  
    oop-demo.h<br>
    oop-test.cpp 

### **C++ 的历史**
  + B 语言（1969）
  + C 语言 (1972)
  + C++ 语言 (1983)<br>
    (new C -> **C with Class** -> C++)
    > C++ 语言是在1983年。他早期不叫 C++，早期叫做 C with Class，所以从这名称就更清楚了啊，他是以 C 语言为基础。
    {: .prompt-info }

  + Java 语言
  + C# 语言

  面向对象语言呢，不是只有 C++, C++ 可能是第一个最大规模的被全世界广泛接受的语言。在他的后面还有很有名的啊，也很多人在使用的是 JAVA 语言跟 C# 以及其他的语言。那事实上，这些语言呢？嗯，精神都一样，关键字也都差不多。所以我们学了一种语言，基本上也就可以了解另外的两三种语言了，当然它细节会有所不同。

### **C++ 演化**
  + `C++ 98 (1.0)`
  + C++ 03 (TR1, Technical Report 1)
  + `C++ 11 (2.0)`
  + C++ 14

  C++ 他这个出现的时间，其实刚刚各位已经看到了1983 年就有了，但是他真正正规化变成有一个国际规范呢，是在 1998 年我们叫 C++ 98。经过了 13 年之后 C++ 11, 2011年才出现 2.0。

  我们学习 C++。可以分为语言的部分，还有标准库的部分，现在几乎学任何一个语言呢，都是这样子，要把这两个分开来。

  在比较早期，也许在 C 的时代呢啊，大家很关注的是语言的部分。当然了，其实你也脱不开标准库的部分 C 也有他的标准库，所以我现在要传达的概念是，像现在语言越来越复杂，所以他们的标准库所给各位的东西也是越来越多，那使用这些标准库。也是一个非常非常重要的事情。我们在这一门课程上，现在的你所看到的这门课程呢，我们**主要谈语言**，我们**用到标准库的部分，只有最简单的**啊，一些东西的输出。

  那事实上，标准库是非常庞大，非常有用的。可以说这么一句话，如果你是一个 C++ programmer 程序员，但是<u>你不怎么去用标准库的话，你不能算是一个合格的好的 C++ 程序员，有生产力的，你可能不能算是</u>。

### **Bibliography (书目誌)**

+ 《C++ Primer 中文版（第5版）》-- Stanley B. Lippman, Josée Lajoie, Barbara E. Moo

![C++ Primer](https://gitee.com/zlj0402/blog_image/raw/master/posts/Languages/cplusplus/Eight_Part_Series/Advanced_Object_Oriented_Programming_in_Cplusplus_Part_1/CplusplusPrimer.jpg)
_《C++ Primer》-- C++ 的第一个编译器的作者所写的书籍_

<img src="https://gitee.com/zlj0402/blog_image/raw/master/posts/Languages/cplusplus/Eight_Part_Series/Advanced_Object_Oriented_Programming_in_Cplusplus_Part_1/CplusplusPrimer.jpg" alt="C++ Primer">


这本书是 C++ 入门的经典书籍，当然现在已经出了第 5 版了。它的内容呢？是从最基础的 C++ 语言开始讲起，然后慢慢地深入到面向对象的部分。

+ 《The C++ PROGRAMMING LANGUAGE》-- C++ 之父

那毕竟我们这课程里面涵盖的是非常还算是非常小的一部分，你还是需要更多的这方面的信息。这边有本书，可以对大家带来很好的帮助。

+ 《Effective C++》-- Scott Meyers
  + 中译版 《Effective C++》-- Scott Meyers, 译者：侯捷

  里面以条款的方式告诉你什么该做。什么不该做？你如果做什么动作，会影响什么样的效率啊？所以这是专家给我们的心得，给我们的经验非常好。

另外呢，有一个很重要的是标准库。标准库现在是很庞大，所以我们也需要好的书籍来帮助学习，我也看过很多很多的这方面的书。

+ 《The C++ STANDARD LIBRARY -- A Tutorial and Reference》

对于初学者来讲，这条路已经很遥远了啊，再深入下去的话，深入到什么？如果你有兴趣，想要看看标准库的源代码。那这种书就很少了啊，就是很很深的部分，有这么一本书:

+ 《STL 源码剖析》-- 侯捷

## 2. 头文件与类的声明
