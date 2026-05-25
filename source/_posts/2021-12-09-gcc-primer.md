---
title: gcc入门
tags: [gcc]
categories: [学习笔记]
comment: true
date: 2021-12-09 23:37:15
---

## 1. gcc简介

GCC是以GPL许可证所发行的自由软件，也是GNU计划的关键部分。GCC的初衷是为GNU操作系统专门编写一款编译器，现已被大多数类Unix操作系统（如[Linux](https://baike.baidu.com/item/Linux/27050)、[BSD](https://baike.baidu.com/item/BSD/3794498)、MacOS X等）采纳为标准的编译器，甚至在微软的Windows上也可以使用GCC。GCC支持多种计算机体系结构芯片，如x86、ARM、MIPS等，并已被移植到其他多种硬件平台。

GCC原名为GNU C语言编译器（GNU C Compiler），只能处理C语言。但其很快扩展，变得可处理C++，后来又扩展为能够支持更多编程语言，如Fortran、Pascal、Objective -C、Java、Ada、Go以及各类处理器架构上的汇编语言等，所以改名GNU编译器套件（GNU Compiler Collection

## 2. gcc分步骤编译程序

编辑如下代码保存为`hello.c`

```C++
#include <stdio.h>

int main(){
  printf("Hello, gcc!\n");
  return 0;
}
```



gcc编译程序主要经过四个过程：

1. 预处理（Pre-Processing）生成 `.i` 文件
2. 编译 （Compiling）成汇编语言，生成 `.s` 文件
3. 汇编 （Assembling）后生成目标文件 `.o` 文件
4. 链接 （Linking），将各个模块的 `.o` 文件链接起来生成一个可执行文件

![img](https://raw.githubusercontent.com/recovxy/PictureBed/master/BlogImg/gcc01.jpg) 

- 预处理阶段

```bash
gcc -E hello.c -o hello.i
```

执行以上命令生成 `hello.i` 文件，本过程主要是处理预处理问题，例如宏的展开问题、文件的包含问题。

- 编译阶段

```bash
gcc -S hello.i
```

执行后生成 `hello.s` 文件，里面全是汇编指令。

- 汇编阶段

```bash
gcc -c hello.s
```

该过程生成 `hello.o` 目标文件，但是不进行链接。

- 链接阶段

```bash
gcc hello.o -o hello
```

链接完会生成 `hello` 可执行文件。



**直接生成可执行文件**

进入 hello.c 的目录下运行：

```shell
gcc hello.c
```

在目录下会生成 `a.out` 文件，然后运行，命令如下

```shell
./a.out
```

## 3. gcc常用选项

来看一下gcc常用选项

| 选项名             | 作用                                    |
| ------------------ | --------------------------------------- |
| -o [file1] [file2] | 将文件 file2 编译成可执行文件 file1     |
| -E                 | 预处理指定的源文件，不进行编译          |
| -S                 | 编译指定的源文件，但是不进行汇编        |
| -c                 | 编译、汇编指定的源文件，但是不进行链接  |
| -Wall              | 使gcc对源文件的代码有问题的地方发出警告 |
| -Idir              | 将dir目录加入搜索头文件的目录路径       |
| -Ldir              | 将dir目录加入搜索库的目录路径           |
| -llib              | 连接lib库                               |
| -g                 | 生成调试信息，该程序可以被调试器调试    |

**注意**：gcc 编译选项会区分大小写。因此`-o`选项和`-O`选项的效果是不一样的。前者表示源文件编译成为可执行文件，后者表示将源文件编译成为可执行文件并且进行一级优化。

使用gcc时可以加上-Wall选项。下面这个例子如果不加上-Wall选项，编译器不会报出任何错误或警告，但是程序的结果却不是预期的：

```cpp
//bad.c
#include<stdio.h>
int main()
{
    printf("the number is %f ",5);  //程序输出了the number is 0.000000，结果错误
    return 0;
 }
```

使用-Wall选项：

> gcc -Wall bad.c -o bad

gcc将输出警告信息：

> warning: format ‘%f’ expects argument of type ‘double’, but argument 2 has type ‘int’ [-Wformat=]
> printf("the number is %f\n",5);

## 4. gcc编译多个文件

假设现在有三个文件：hello.c hello.h main.c ,三个文件的内容如下：

```cpp
// hello.c
#include<stdio.h>
#include"hello.h"
void printHello()
{
        printf("hello world!\n");
}

//main.c
#include<stdio.h>
#include"hello.h"
int main()
{
        printHello();
        return 0;
}

//hello.h
//仅包含函数声明
#ifndef _HELLO_
#define _HELLO_
void printHello();
#endif
```

编译这三个文件，可以一次编译：

> gcc hello.c main.c -o main 生成可执行文件main

也可以独立编译：

> gcc -Wall -c main.c -o main.o
> gcc -Wall -c hello.c -o hello.o
> gcc -Wall main.o hello.o -o main

独立编译的好处是，当其中某个模块发送改变时，只需要编译该模块就行，不必重新编译所有文件，这样可以节省编译时间。

## 5. 使用外部库

在使用C语言和其他语言进行程序设计的时候，我们需要头文件来提供对常数的定义和对系统及库函数调用的声明。库文件是一些预先编译好的函数集合，那些函数都是按照可重用原则编写的。它们通常由一组互相关联的可重用原则编写的，它们通常由一组互相关联的用来完成某项常见工作的函数构成。使用库的优点在于：

- 模块化的开发
- 可重用性
- 可维护性

库又可以分为静态库与动态库：

- 静态库（.a）：程序在编译链接的时候把库的代码链接到可执行文件中。程序运行的时候将不再需要静态库。静态库比较占用磁盘空间，而且程序不可以共享静态库。运行时也是比较占内存的，因为每个程序都包含了一份静态库。
- 动态库（.so或.sa）：程序在运行的时候才去链接共享库的代码，多个程序共享使用库的代码，这样就减少了程序的体积。

一般头文件或库文件的位置在：

- /usr/include及其子目录底下的include文件夹
- /usr/local/include及其子目录底下的include文件夹
- /usr/lib
- /usr/local/lib
- /lib

## 6. 生成静态库

为了生成.a文件，我们需要先生成.o文件。下面这行命令将我们的hello.o打包成静态库libhello.a：

> ar rcs libhello.a hello.o

ar是gun归档工具，rcs表示replace and create，如果libhello之前存在，将创建新的libhello.a并将其替换。

- r --- 讲文件插入备存文件中
- c --- 建立备存文件
- s --- 索引

然后就可以这样来使用静态库libhello.a

> gcc -Wall main.c libhello.a -o main

还有另外一种使用方式：

> gcc -Wall -L. main.c -o main -lhello 【lhello 是 libhello的缩写】

其中  **-L.** 表示库文件的位置在当前目录下，由于libhello.a是我们自己生成的，并存放在当前录下下，所以需要加上-L.选项。默认库文件是在系统的目录下进行搜索。同样的，-I.选项用于头文件的搜索。

## 7. 生成动态（共享）库

生成一个共享库，名称的规则是libxxx.so。将刚才hello.o生成libhello.so的命令为：

> gcc -shared -fPIC hello.o -o libhello.so

生成了共享库之后，可以这样来使用共享库：

> gcc -Wall main.o -o main -L. -lhello

该命令与使用静态库的命令相同，但是在共享库与静态库共存的情况下，优先使用共享库。

共享库有时候并不不在当前的目录下，为了让gcc能够找得到共享库，有下面几种方法：

1. 拷贝.so文件到系统共享库路径下，一般指/usr/lib
2. 在~/.bash_profile文件中，配置LD_LIBRARY_PATH变量
3. 配置/etc/ld.so.conf，配置完成后调用ldconfig更新ld.so.cache

其中，shared选项表示生成共享库格式。fPIC表示产生位置无关码（position independent code），位置无关码表示它的运行、加载与内存位置无关，可以在任何内存地址进行加载。

## 8. 库的搜索路径

库的搜索路径遵循几个搜索原则：从左到右搜索-I -l指定的目录，如果在这些目录中找不到，那么gcc会从由**环境变量指定的目录**进行查找。头文件的环境变量是**C_INCLUDE_PATH**,库的环境变量是**LIBRARY_PATH**.如果还是找不到，那么会从**系统指定指定的目录**进行搜索。

## 9.gcc和g++的区别

- gcc 和 g++ 都是GNU（组织）的一个编译器。
- 误区—：gcc 只能编译C代码，g++只能编译 C++代码。两者都可以，请注意：
  - 后缀为.c的，gcc 把它当作是 C 程序，而g++ 当作是 C++
    程序
  - 后缀为.cpp的，两者都会认为是  C++ 程序，C＋＋的语法规则更加严谨一些
  - 编译阶段，g++ 会调用 gcc，对于  C++ 代码，两者是等价的，但是因为 gcc 命令不能自动和  C++ 程序使用的库链接，所以通常用  C++来完成链接，为了统一起见，干脆编译／链接统统用 g++了，这就给人一种错觉，好像 cpp 程序只能用 g++ 似的
- 误区二：gcc 不会定义__cplusplus宏，而g++会
  - 实际上，这个宏只是标志着编译器将会把代码按 C 还是 C++ 语法来解释
  - 如上所述，如果后缀为.c，并且采用 gcc 编译器，则该宏就是未定义的，否则，就是已定义

- 误区三：编译只能用 gcc，链接只能用 g++
  - 口严格来说，这句话不算错误，但是它混淆了概念，应该这样说：编译可以用gcc／g++，而链接可以用 g++ 或者 gcc -lstdc++
  - gcc 命令不能自动和 C++ 程序使用的库联接，所以通常使用 g++ 来完成链接。但在编译阶段，g++ 会自动调用 gcc，二者等价
