---
layout: post
title: C++的IO类
date: 2021-11-03 22:43:21
categories: [学习记录]
tags: [C++, 学习]
comments: true
---

## 引言

C++不直接处理输入和输出，而是通过标准库中的类型处理IO。IO的设备可以是文件、控制台、string。为了支持这些不同类的IO处理操作，C++主要定义了三种IO类型，分别被包含在`iostream`、`fstream`、`sstream`头文件中。

为了支持使用宽字符的语言，标准库定义了一组类型和对象操纵wchar_t类型的数据。

以下是这三种IO库类型以及头文件：

- iostream头文件
  - istream（宽字符版本wistream），从流读取数据。
  - ostream （宽字符版本wostream），向流写入数据。
  - iostream（宽字符版本wiostream），读写流。
- fstream头文件
  - ifstream（宽字符版本wifstream），从文件读取数据。
  - ofstream （宽字符版本wofstream），向文件写入数据。
  - fstream（宽字符版本wfstream），读写文件。
- sstream头文件
  - istringstream（宽字符版本wistringstream），从string读取数据。
  - ostringstream （宽字符版本wostringstream），向string写入数据。
  - stringstream（宽字符版本wstringstream），读写string。

设备类型和字符大小不会影响我们要执行的IO操作。得益于继承机制，以上类型都可以使用`>>`、`<<`运算符以及`getline()`函数。

## IO对象无拷贝或赋值

```C++
ofstream out1, out2;
out1 = out2;	//错误：不能对流对象赋值
ofstream print(ofstream);	//错误：不能初始化ofstream参数
out2 = print(out);		//错误：不能拷贝流对象
```

由于无法拷贝IO对象，因此**不能将形参或返回类型设置为流类型**。通常使用**引用**方式传递和返回流。但传递和返回的引用不能是`const`的，因为读写IO对象会改变IO对象的状态，也就是改变了IO对象。

## IO对象的条件状态

IO类定义了一些函数和标志位，帮助我们检查和操纵流的状态：

- `strm::iostate`是一种类型，这种类型就像一串二进制位串，每个二进制位串指出了流的状态。(strm为引言中的任意一种IO类型)。
- `strm::badbit`用来指出流已崩溃
- `strm::failbit`用来指出一个IO操作失败了
- `strm::eofbit`用来指出流到达了文件结束
- `strm::goodbit`用来指出流位处于错误状态。此值保证为0。
- `eof()`函数用于在IO对象上调用，如`cin.eof()`。如果流检测到`eof`（文件结束标志），该函数返回true。
- `fail()`函数使用同上。如果流处于崩溃或IO操作失败的状态（即`failbit`或`badbit`置位），返回true。
- `bad()`函数使用同上。如果流处于崩溃状态，返回true。
- `good()`函数使用同上。如果流处于有效状态，返回true。
- `clear()`函数使用同上。将IO对象中的所有条件状态为复位，流的状态变为有效状态，返回void。
- `setstate(flags)`将IO对象的状态为按flags（类型为`strm::iostate`）指示的那样置位。
- `rdstate()`函数用于在IO对象上调用,读取IO对象的状态位，返回类型为`strm::iostate`。

## 管理输出缓冲

每一个输出流管理一个缓冲区，用来保存程序读写的数据。例如：

```C++
cout << "Hello World!";
```

串"Hello World!"可能被立即打印出来，也可能被操作系统保存在缓冲区，随后打印。

导致缓冲区刷新（即，数据真正写到输出设备或文件）的原因有很多：

- 程序正常结束，自动刷新。
- 缓冲区满时，自动刷新。
- 可以使用操作符`endl`、`flush`、`ends`手动刷新缓冲区（只作用一次输出）。

```C++
cout << "1" << endl; //字符串后添加换行，然后刷新缓冲区
cout << "2" << flush; //仅刷新缓冲区
cout << "3" << ends;  //字符串后添加一个空字符，然后刷新缓冲区。
```

- 通过操作符unitbuf设置自动刷新。不同于`endl`、`flush`、`ends`只作用于一次输出，设置了unitbuf后的输出流每输出一次都会自动刷新缓冲区。

```C++
cout << unitbuf;//下面的语句每执行一次输出，就刷新一次缓冲区。
cout << "1"; //输出"1"，自动刷新缓冲区
cout << "2"; //输出"2"，自动刷新缓冲区
cout << "3"; //输出"3"，自动刷新缓冲区
cout << "4"; //输出"4"，自动刷新缓冲区
...
cout << "nounitbuf"; //回到流默认的缓冲方式
```

- 关联流。读写被关联的流时，关联到的流的缓冲区会被刷新（`tie()`函数括号里面的是关联到的流，调用`tie()`的流是被关联的流）。cout 和 cin默认关联在一起，使用cin读取数据时，cout的缓冲区被刷新。

```C++
cout << "Fuck you!"; //没有指定操作符，cout默认不刷新，该语句执行完后"Fuck you!"可能立即被输出到屏幕，也可能稍后被输出。
int i;
cin >> i; //cout的缓冲区被刷新，此时"Fuck you!"一定已经真正输出（可能在之前就已经真正输出，此时刷新缓冲区等于什么都没做）。
```

使用`tie()`函数关联流和解除关联：

```C++
cin.tie(&cout); //有参数的tie(),参数为指向流的指针，且指针不为空，此时建立关联。
cin.tie(nullptr); //有参数的tie(),且指针为空，此时解除cin和其他流的关联。
cin.tie(); //无参tie()，返回指向cin当前关联到的流的指针。
```

## 文件输入输出

## 创建文件流

前面说过，所有IO类型都可以使用`>>`、 `<<`与`getline()`，除此之外，文件IO还有一些特有的操作。

创建文件流：



```C++
fstream fstrm1; //创建未绑定文件的文件流
fstream fstrm2(s1); //创建绑定到指定文件s1的文件流(自动调用open())。s1是string或指向c风格字符串的指针。
//fstream fstrm3(s2, mode); 与第二条语句类似，但指定打开文件的模式。
```

Note:

> 当一个fstream的作用域内的代码执行完毕，fstream关联的文件被**自动关闭**，即fstream对象被销毁时，close()会自动调用。

## open和close

使用open打开文件，close关闭文件。对一个已经打开的文件调用open会失败，并且failbit被置位。

```C++
string file1 = "qq.dat";
ifstream ifs;
ifs.open(file1);
//读取操作
ifs.close();
```

## 文件模式mode

常用的文件模式mode如下：

- in 只读方式打开文件
- out 以写方式打开文件
- app 每次写操作在文件末尾进行
- ate 打开文件后立即定位到文件末尾
- trunc 截断文件，即输出会覆盖文件中的原有数据。
- binary 以二进制方式打开文件

同时指定多个模式时使用`|`分隔:

```C++
ofstream ofs("file1", ofstream::out | ofstream::app);
```

Note：

> `out`模式隐含`trunc`即覆盖原文件，若要在原文件末尾添加数据，则需要显式指明`app`模式。
>
> 若没有指定任何模式，则使用默认模式

## string IO

stringstream独有的操作如下：

```C++
sstream strm;   //sstream为sstream头文件中定义的类型，具体可以是istringstream等
sstream strm(s); //建立一个sstream对象，保存字符串s的一个拷贝
strm.str(); //返回strm保存的string的拷贝
strm.str(s); //将string s拷贝到strm
```