---
layout: post
title: "对于C++ const的一些用法总结"
date: 2022-08-07
description: "const的简单总结"
tag: Cplusplus
author: bugthree
---
# const的一些简单总结
## 1. const 使用的三个位置
const 使用的三个位置指的是：
- 形参(input)：该形参不能改变
- 函数(只读函数)
- 返回值：函数返回值不能改变

## 2. const 的顶层和底层讨论
指针本事就是一个对象，它有可以指向另外一个对象，所以指针本身是const和本身指向的对象是const就是两个独立的问题；
- 顶层const：指针本身是常量
- 底层const：指针指向的对象是常量

1. 顶层const可以表示任意的对象是常量。这一点对于任何数据类型都适用，如算术类型、类、指针等。
2. 底层const则与指针和引用等复合类型的基本类型有关 
3. 特殊情况：指针类型既可以是顶层也可以是底层
```dotnetcli
int i = 0;
int *const p1 = &i;// 不能改变p1的值，顶层const
const int ci = 42;// 不能改变ci的值，顶层const
const int *p2 = &ci;// const从左向右看与指针近，这是一个底层const，允许改变p2的值

const int *const p3 = p2;// 靠右的是顶层，靠左的是底层
const int &r = ci;// 用于声明引用的const都是底层const
```

当执行对象拷贝的操作，常量是顶层const还是底层const区别明显？
- 对于顶层const并无影响
- 对于底层const，
    - 拷入和拷出的对象必须是相同的底层const
    - 或者两个对象的数据类型必须能够转换，一般而言，非常量可以转换成常量，反之不行
```dotnetcli
int *p = p3;// 错，p3是顶层，也是底层，而p不是底层
p2 = p3；// 正确，两者都是底层
p2 = &i; // 正确，int* 能转换成const int*
int &r = ci;//错误，普通的int&不能绑定到int常量上
const int &r2 = i;// 正确const int& 可以绑定到一个普通int上
```
