---
layout: post
title: "创建型:Factory Method"
date: 2022-08-15
description: "Factory Method"
tag: design patterns
author: bugthree
---

## 4. 对象创建型设计模式
通过“对象创建” 模式绕开new，来避免对象创建（new）过程中所导致的紧耦合（依赖具体类），从而支持对象创建的稳定。它是接口抽象之后的第一步工作
- Factory Method
- Abstract Factory
- Prototype
- Builder
### 4.1 Factory Method(简单工厂模式)
#### 4.1.1 模式动机
- 在软件系统中，经常面临着创建对象的工作；由于需求的变化，需要创建的对象的具体类型经常变化。
- 如何应对这种变化？如何绕过常规的对象创建方法(new)，提供一种“封装机制”来避免客户程序和这种“具体对象创建工作”的紧耦合？

考虑一个简单的软件应用场景，一个软件系统可以提供多个外观不同的按钮（如圆形按钮、矩形按钮、菱形按钮等）， 这些按钮都源自同一个基类，不过在继承基类后不同的子类修改了部分属性从而使得它们可以呈现不同的外观，如果我们希望在使用这些按钮时，不需要知道这些具体按钮类的名字，只需要知道表示该按钮类的一个参数，并提供一个调用方便的方法，把该参数传入方法即可返回一个相应的按钮对象，此时，就可以使用简单工厂模式。
#### 4.1.2 模式定义
- 定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method使得一个类的实例化延迟（目的：解耦，手段：虚函数）到子类。 ——《设计模式》GoF

简单工厂模式(Simple Factory Pattern)：又称为静态工厂方法(Static Factory Method)模式，它属于类创建型模式。在简单工厂模式中，可以根据参数的不同返回不同类的实例。简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。
#### 4.1.3 模式结构
简单工厂模式包含如下角色：
- Factory：工厂角色
    - 工厂角色负责实现创建所有实例的内部逻辑
- Product：抽象产品角色
    - 抽象产品角色是所创建的所有对象的父类，负责描述所有实例所共有的公共接口
- ConcreteProduct：具体产品角色
    - 具体产品角色是创建目标，所有创建的对象都充当这个角色的某个具体类的实例。

#### 4.1.4 模式分析
1. 将对象的创建和对象本身业务处理分离可以降低系统的耦合度，使得两者修改起来都相对容易。
2. 在调用工厂类的工厂方法时，由于工厂方法是静态方法，使用起来很方便，可通过类名直接调用，而且只需要传入一个简单的参数即可，在实际开发中，还可以在调用时将所传入的参数保存在XML等格式的配置文件中，修改参数时无须修改任何源代码。
3. 简单工厂模式最大的问题在于工厂类的职责相对过重，增加新的产品需要修改工厂类的判断逻辑，这一点与开闭原则是相违背的。
4. 简单工厂模式的要点在于：当你需要什么，只需要传入一个正确的参数，就可以获取你所需要的对象，而无须知道其创建细节。
#### 4.1.5 简单工厂模式的优点
1. 工厂类含有必要的判断逻辑，可以决定在什么时候创建哪一个产品类的实例，客户端可以免除直接创建产品对象的责任，而仅仅“消费”产品；简单工厂模式通过这种做法实现了对责任的分割，它提供了专门的工厂类用于创建对象。
2. 客户端无须知道所创建的具体产品类的类名，只需要知道具体产品类所对应的参数即可，对于一些复杂的类名，通过简单工厂模式可以减少使用者的记忆量。
3. 通过引入配置文件，可以在不修改任何客户端代码的情况下更换和增加新的具体产品类，在一定程度上提高了系统的灵活性。
#### 4.1.6 简单工厂模式的缺点
1. 由于工厂类集中了所有产品创建逻辑，一旦不能正常工作，整个系统都要受到影响。
2. 使用简单工厂模式将会增加系统中类的个数，在一定程序上增加了系统的复杂度和理解难度。
3. 系统扩展困难，一旦添加新产品就不得不修改工厂逻辑，在产品类型较多时，有可能造成工厂逻辑过于复杂，不利于系统的扩展和维护。
4. 简单工厂模式由于使用了静态工厂方法，造成工厂角色无法形成基于继承的等级结构。
#### 4.1.7 要点总结
- Factory Method模式用于隔离类对象的使用者和具体类型之间的耦合关系。面对一个经常变化的具体类型，紧耦合关系(new)会导致软件的脆弱。
- Factory Method模式通过面向对象的手法，将所要创建的具体对象工作延迟到子类，从而实现一种扩展（而非更改）的策略，较好地解决了这种紧耦合关系。
- Factory Method模式解决“单个对象”的需求变化。缺点在于要求创建方法/参数相同。
#### 4.1.8 代码分析

文本分割器案例，代码版本1：
问题：
- 面向接口设计原则：一个对象类型往往应该声明为抽象类，最好不要声明为具体类


```

class MainForm : public Form
{
	TextBox* txtFilePath;
	TextBox* txtFileNumber;
	ProgressBar* progressBar;

public:
	void Button1_Click(){

		ISplitter * splitter=
            new BinarySplitter();//依赖具体类
        
        splitter->split();

	}
};


class ISplitter{// 抽象基类
public:
    virtual void split()=0;
    virtual ~ISplitter(){}
};

// 需求变动，有很多不同功能形式的分割
// 下面为具体实现：
class BinarySplitter : public ISplitter{
    
};

class TxtSplitter: public ISplitter{
    
};

class PictureSplitter: public ISplitter{
    
};

class VideoSplitter: public ISplitter{
    
};

```

文本分割器案例，代码版本2：

```

class MainForm : public Form
{
    SplitterFactory*  factory;//工厂

public:
    
    MainForm(SplitterFactory*  factory){
        this->factory=factory;
    }
    
	void Button1_Click(){

        // 面向接口设计，这里不要像代码版本1，作为具体实现，而应该用指针指向抽象类,
        // 代码1改为下面：
        // ISplitter * splitter=
        //    new BinarySplitter();
        // ISplitter * splitter= 此时这里已经是依赖抽象了，但new BinarySplitter(); 还是一个依赖具体实现

        // 因此，引出工厂模式
        // 绕开new，依赖抽象
		ISplitter * splitter=
            factory->CreateSplitter(); //多态new

        splitter->split();
	}
};

//具体类
class BinarySplitter : public ISplitter{
    
};

class TxtSplitter: public ISplitter{
    
};

class PictureSplitter: public ISplitter{
    
};

class VideoSplitter: public ISplitter{
    
};

//具体工厂
class BinarySplitterFactory: public SplitterFactory{
public:
    virtual ISplitter* CreateSplitter(){
        return new BinarySplitter();
    }
};

class TxtSplitterFactory: public SplitterFactory{
public:
    virtual ISplitter* CreateSplitter(){
        return new TxtSplitter();
    }
};

class PictureSplitterFactory: public SplitterFactory{
public:
    virtual ISplitter* CreateSplitter(){
        return new PictureSplitter();
    }
};

class VideoSplitterFactory: public SplitterFactory{
public:
    virtual ISplitter* CreateSplitter(){
        return new VideoSplitter();
    }
};

// 工厂类
//抽象类
class ISplitter{
public:
    virtual void split()=0;
    virtual ~ISplitter(){}
};


//工厂基类
class SplitterFactory{
public:
    virtual ISplitter* CreateSplitter()=0;
    virtual ~SplitterFactory(){}
};

```