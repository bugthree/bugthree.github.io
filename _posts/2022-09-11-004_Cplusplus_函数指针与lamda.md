---
layout: post
title: "函数指针与lamda"
date: 2022-09-11
description: "对函数指针与lamda的简单总结"
tag: Cplusplus
author: bugthree
---

## 1. 函数指针与lamda
### 1.1 C语言风格的函数指针
1. 定义方式
`void(*function)() = Print; //很少用，一般用auto关键字`

2. 函数指针的使用
- 无参数的函数指针

```

void Print() {
    std::cout << "hello，world" << std::endl;
}
int main() {
    //void(*function)() = Print； 正常写法，但一般用auto就可以了
    auto function = Print();    //ERROR!，auto无法识别void类型
    auto function = Print;  //OK!，去掉括号就不是在调用这个函数，而是在获取函数指针，得到了这个函数的地址。就像是带了&取地址符号一样"auto function = &Print;""(隐式转换)。
    function();//调用函数
    //这里函数指针其实也用到了解引用（*），这里是发生了隐式的转化，使得代码看起来更加简洁明了！
}

```

`//输出：hello,world`


3. 对于有参数的函数指针，在使用的时候传上参数即可

```

void Print(int a) {
    std::cout << a << std::endl;
}
int main() {
    auto temp = Print;  //正常应该是 void(*temp)(int) = Print,太过于麻烦，用auto即可
    temp(1);    //在用函数指针的时候也传参数进去就可以正常使用了
}

```

- 也可以用typedef或者using来使用函数指针 

```

#include<iostream>

void HelloWorld()
{
    std::cout << "Hello World!" << std::endl;
}
int main()
{
    typedef void(*HelloWorldFunction)(); 

    HelloWorldFunction function = HelloWorld;
    function();

    std::cin.get();
}

```

4. 为什么要首先使用函数指针
- 如果需要将一个函数作为另一个函数的形参，那么就要需要函数指针 .

```

void Print(int val) {
    std::cout << val << std::endl;
}

//下面就将一个函数作为形参传入另一个函数里了
void ForEach(const std::vector<int>& values, void(*function)(int)) {
    for (int temp : values) {
        function(temp); //就可以在当前函数里用其他函数了
    }
}

int main() {
    std::vector<int> valus = { 1, 2, 3, 4, 5 };
    ForEach(values, Print); //这里就是传入了一个函数指针进去！！！！
}

```

5. 优化：lambda
- lambda本质上是一个普通的函数，只是它不像普通函数这样声明，它是我们的代码在过程中生成的，用完即弃的函数，不算一个真正的函数，是匿名函数 。
- 格式：[] ({形参表}) {函数内容}

```

void ForEach(const std::vector<int>& values, void(*function)(int)) {
    for (int temp : values) {
        function(temp);     //正常调用lambda函数
    }
}

int main() {
    std::vector<int> valus = { 1, 2, 3, 4, 5 };
    ForEach(values, [](int val){ std::cout << val << std::endl; });     //如此简单的事就交给lambda来解决就好了
}

```

### 1.2 lamda

1. lambda本质上是一个匿名函数。 用这种方式创建函数不需要实际创建一个函数 ，它就像一个快速的一次性函数 。 lambda更像是一种变量，在实际编译的代码中作为一个符号存在，而不是像正式的函数那样。

2. 使用场景：
- 在我们会设置函数指针指向函数的任何地方，我们都可以将它设置为lambda 

3. lambda表达式的写法(使用格式)：[]( {参数表} ){ 函数体 }
- 中括号表示的是捕获，作用是如何传递变量 lambda使用外部（相对）的变量时，就要使用捕获。

如果使用捕获,则：
- 添加头文件： #include <functional>
- 修改相应的函数签名 std::function <void(int)> func替代 void(*func)(int)
- 捕获[]使用方式：

- [=]，则是将所有变量值传递到lambda中
- [&]，则是将所有变量引用传递到lambda中
- [a]是将变量a通过值传递，如果是[&a]就是将变量a引用传递
- 它可以有0个或者多个捕获 

```

//If the capture-default is `&`, subsequent simple captures must not begin with `&`.
struct S2 { void f(int i); };
void S2::f(int i)
{
    [&]{};          // OK: by-reference capture default
    [&, i]{};       // OK: by-reference capture, except i is captured by copy
    [&, &i] {};     // Error: by-reference capture when by-reference is the default
    [&, this] {};   // OK, equivalent to [&]
    [&, this, i]{}; // OK, equivalent to [&, i]
}


//If the capture-default is `=`, subsequent simple captures must begin with `&` or be `*this` (since C++17) or `this` (since C++20). 
struct S2 { void f(int i); };
void S2::f(int i)
{
    [=]{};        // OK: by-copy capture default
    [=, &i]{};    // OK: by-copy capture, except i is captured by reference
    [=, *this]{}; // until C++17: Error: invalid syntax
                  // since C++17: OK: captures the enclosing S2 by copy
    [=, this] {}; // until C++20: Error: this when = is the default
                  // since C++20: OK, same as [=]
}

```

[详情参考：](https://en.cppreference.com/w/cpp/language/lambda)

```

#include <iostream>
#include <vector>
#include <functional>
void ForEach(const std::vector<int>& values, void(*function)(int)) {
    for (int temp : values) {
        function(temp);     //正常调用lambda函数
    }
}

int main() {
    std::vector<int> valus = { 1, 2, 3, 4, 5 };
    //函数指针的地方都可以用auto来简化操作，lambda亦是
    //这样子来定义lambda表达式会更加清晰明了
    auto lambda = [](int val){ std::cout << val << std::endl; }
    ForEach(values, lambda);    
}

```

```

//lambda可以使用外部（相对）的变量，而[]就是表示打算如何传递变量
#include <functional>   //要用捕获就必须要用C++新的函数指针！
//新的函数指针的签名有所不同！
void ForEach(const std::vector<int>& values, const std::function<void(int)>& func) {
    for (int temp : values) {
        func(temp);     
    }
}

int main() {
    std::vector<int> valus = { 1, 2, 3, 4, 5 };
    //注意这里的捕获必须要和C++新带的函数指针关联起来！！！
    int a = 5;  //如果lambda需要外部的a向量
    //则在捕获中写入a就好了
    auto lambda = [a](int val){ std::cout << a << std::endl; }
    ForEach(values, lambda);    
}

```

我们有一个可选的修饰符mutable，它允许lambda函数体修改通过拷贝传递捕获的参数。若我们在lambda中给a赋值会报错，需要写上mutable 。

```

int a = 5;
auto lambda = [=](int value) mutable { a = 5; std::cout << "Value: " << value << a << std::endl; };

```

另一个使用lambda的场景find_if
- 我们还可以写一个lambda接受vector的整数元素，遍历这个vector找到比3大的整数，然后返回它的迭代器，也就是满足条件的第一个元素。
- find_if是一个搜索类的函数，区别于find的是：它可以接受一个函数指针来定义搜索的规则，返回满足这个规则的第一个元素的迭代器。这个情况就很适合lambda表达式的出场了 

```

#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> values = { 1, 5, 3, 4, 2 };
    //下面就用到了lambda作为函数指针构成了find_it的规则
    auto it = std::find_if(values.begin(), values.end(), [](int value) { return value > 3; });  //返回第一个大于3的元素的迭代器 
    std::cout << *it << std::endl;  //将其输出
}

```

