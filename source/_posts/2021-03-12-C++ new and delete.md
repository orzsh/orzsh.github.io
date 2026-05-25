---
layout: post
title: C++的new与delete
date: 2021-3-12 14:23:05
categories: [学习记录]
tag: [C/C++]

comments: true
---



我们先定义一个complex类

```c++
class complex{
private:
    double m_real;
    double m_imag;
...
public:
	Complex(...){...} //构造函数，创建对象开始就自动调用
    ~Complex(...){...} //析构函数，当这个对象的“生命结束”时自动调用
};
```

## new

现在我们要创建一个complex指针并赋初值

```C++
Complex* pc=new Complex(1,2);
```

编译器会将这个过程转化为以下三步：

```C++
① void* mem = operator new(sizeof(Complex)); //先分配内存给一个空指针,内部调用了malloc(n)
② pc = static_cast<Complex*>(mem);	//再将这个空指针的类型转换为Complex*
③ pc->Complex::Complex(1,2); //调用构造函数
```

## delete

现在我们要释放这块内存

```C++
delepe pc;
```

编译器会转化为以下两步：

```C++
① Complex::~Complex(pc); //析构函数
② operator delete(pc); //释放内存，内部调用了free(pc);
```

## delete[]

当我们声明一个数组后,释放它时为了避免内存泄漏就应使用delete[]，尽管不一定会造成内存泄漏，但我们使用delete[] 是一个好习惯。

```C++
Complex* p = new Copmpex[3];
delete[] p; //实际调用了3（数组的大小）次析构函数
```

