---
layout: post
title: "对于C++引用的一些用法总结"
date: 2022-08-07
description: "引用的简单总结"
tag: Cplusplus
author: bugthree
---

## 1. 对于C++引用的一些用法总结
引用就是一个变量的别名，它必须初始化，具体可参考c++primer。
它最重要的用法就是再一个函数中，作为形参，或者作为函数的返回值使用，

1. pass by reference 与它对应的是 pass by value
    - 能用pass by reference 能用一定要用，比如
        - 因为这样效率高，比如传入参数是一个类类型，那这个数据就可能很大，reference就是一个指针，这样不用将所有数据压入栈中，提高效率
        - 传引用实际是对实参进行操作，而传值是形参对**实参拷贝的副本操作**，再函数调用时，要为形参分配存储空间。若传递的是对象，还需要调用构造函数
        - pass by pointer，与引用有类似的效果，但是再被调函数中同样要给形参分配存储空间，且需要重复使用指针变量名的形式进行运算
        - 若想将传递给函数中的数据不再函数中改变，就加const（能加就加，不加容易出错）
    - 对于确定类型的可以使用pass by value，比如形参是一个 unsigned 

2. return by reference 和 return by value
    - return by reference 在内存中不产生返回值的副本
    - 但 return by reference 有时候不能用的时候，一定不能用
        - 不能返回局部变量的的引用
        - 不能返回函数内部new分配的内存引用
