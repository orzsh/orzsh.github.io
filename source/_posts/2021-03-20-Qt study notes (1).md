---
layout: post
title: Qt学习笔记（一）
date: 2021-03-20 23:21:05
categories: [学习记录]
tag: [Qt, C/C++]
comments: true
---

# 1 Qt简单介绍

## 1.1 什么是Qt

Qt是一个**跨平台**的C++**图形用户界面应用程序框架**。它为应用程序开发者提供建立艺术级图形界面所需的所有功能。它是完全面向对象的，很容易扩展，并且允许真正的组件编程。

## 1.2 支持的平台

l  Windows – XP、Vista、Win7、Win8、Win2008、Win10

l  Uinux/X11 – Linux、Sun Solaris、HP-UX、Compaq Tru64 UNIX、IBM AIX、SGI IRIX、FreeBSD、BSD/OS、和其他很多X11平台

l  Macintosh – Mac OS X

l  Embedded – 有帧缓冲支持的嵌入式Linux平台，Windows CE

## 1.3 Qt的优点

- l  跨平台，几乎支持所有的平台

- l  接口简单，容易上手，学习QT框架对学习其他框架有参考意义。 

- l  一定程度上简化了内存回收机制 

- l  开发效率高，能够快速的构建应用程序。

- l  有很好的社区氛围，市场份额在缓慢上升。

- l  可以进行嵌入式开发。

# 2 Qt

## 2.1 使用向导创建

打开Qt Creator 界面选择 New Project或者选择菜单栏 【文件】-【新建文件或项目】菜单项

![img](/assets/Qt.assets/01_02.jpg)

【Location】弹出New File or Project对话框，选择Qt Widgets Application

![img](/assets/Qt.assets/01_03.jpg)

选择【Choose】按钮，弹出如下对话框

![img](/assets/Qt.assets/01_04.jpg)

【Build System】选择默认的qmke即可

![img](/assets/Qt.assets/01_05.jpg)

【Detials】Base class 根据需要选择，QMainWindow（带菜单栏）和QDialog（带对话框）都是继承自QWidget（精简版）。

**QWidget**类是所有用户界面对象的基类。 窗口部件是用户界面的一个原子：它从窗口系统接收鼠标、键盘和其它事件，并且在屏幕上绘制自己的表现。每一个窗口部件都是矩形，并且它们按Z轴顺序排列的。一个窗口部件可以被它的父窗口部件或者它前面的窗口部件盖住一部分。

**QMainWindow** 类提供一个有菜单条、锚接窗口（例如工具条）和一个状态条的主应用程序窗口。主窗口通常用在提供一个大的中央窗口部件（例如文本编辑或者绘制画布）以及周围菜单、工具条和一个状态条。QMainWindow常常被继承，因为这使得封装中央部件、菜单和工具条以及窗口状态变得更容易。继承使创建当用户点击菜单项或者工具条按钮时被调用的槽成为可能。你也可以使用Qt设计器来创建主窗口。

**QDialog**类是对话框窗口的基类。对话框窗口是主要用于短期任务以及和用户进行简要通讯的顶级窗口。QDialog可以是模式的也可以是非模式的。QDialog支持扩展性并且可以提供返回值。它们可以有默认按钮。QDialog也可以有一个QSizeGrip在它的右下角，使用setSizeGripEnabled()。**QDialog** 是最普通的顶级窗口。不被嵌入到一个父窗口部件的窗口部件被叫做顶级窗口部件。通常情况下，顶级窗口部件是有框架和标题栏的窗口（尽管如果使用了一定的窗口部件标记，创建顶级窗口部件时也可能没有这些装饰。）在Qt中，QMainWindow和和不同的QDialog的子类是最普通的顶级窗口。

如果是顶级对话框，那就基于**QDialog**创建，如果是主窗体，那就基于**QMainWindow**，如果不确定，或者有可能作为顶级窗体，或有可能嵌入到其他窗体中，则基于**QWidget**创建。当然了，实际中，你还可以基于任何其他部件类来派生。看实际需求了，比如QFrame、QStackedWidget等等。

![img](/assets/Qt.assets/01_06.jpg)

【Translation】可有可无吧，直接下一步

![img](/assets/Qt.assets/01_07.jpg)

【Kits】选择编译套件

![img](/assets/Qt.assets/01_08.jpg)

【Summay】点击完成

![img](/assets/Qt.assets/01_09.jpg)

## 2.2 .pro工程文件

```C++
QT		+= core gui //包含的模块
greaterThan(QT_MAJOR_VERSION, 4): QT += widgets //大于Qt4版本 才包含widget模块
TARGET = QtFirst //应用程序名 生成的.exe程序名称
TEMPLATE = app //模板类型  应用程序模板
SOURCES += main.cpp\  //源文件
        mywidget.cpp \
HEADERS +=mywidget.h \    //头文件
```

## 2.3 main.cpp

```C++
#include "mywidget.h"
#include <QApplication>

int main(int argc, char *argv[])
{
   	//QApplication 应用程序类 初始化我们的应用程序
    QApplication a(argc, argv);
    //创建应该窗口控件
    MyWidget w;
    //显示一个窗口 hide隐藏，且窗口默认隐藏
    w.show();
    //a.exec() 主事件循环（带阻塞 等待用户操作界面）
    return a.exec();
}
```

## 2.4widget.h

```C++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>

class Widget : public QWidget
{
 Q_OBJECT//让Widget支持信号和槽机制

 public:
  Widget(QWidget *parent = 0);
  ~Widget();
};

#endif // WIDGET_H
```

## 2.5 widget.cpp

```C++
#include "widget.h"

Widget::Widget(QWidget *parent)
 : QWidget(parent)
 {
 //界面的设计是在 窗口控件的构造函数中设计
 }

Widget::~Widget()
{

}
```

# 3 第一个程序

写在wedget的构造函数中：

```C++
Widget::Widget(QWidget *parent)
 : QWidget(parent)
{
 	//this 代表当前主窗口
 	//设置标题
     this->setWindowTitle("第一个窗口");
     //固定窗口（不可拖动）
     //this‐>setFixedSize(800,600);
     //设置窗口大小(可拖动)
     this‐>resize( 800 , 600 );
     
     //在窗口上放一个按钮
     //创建一个button控件
     //parent父对象为this 表明 主窗口 将来接管button
     //因为我希望将button放到当前窗口中 this代表当前窗口
     QPushButton *button1 = new QPushButton("戳我呀",this);
     
     QPushButton *button2 = new QPushButton("点我呀",this);
     //默认控件会显示到主窗口的左上方
     //移动按钮
     button1‐>move( 400 , 300 );
 }  
```

# 4 对象树

在创建QObject对象时，可以提供一个其父对象，我们创建的这个QObject对象会自动添加

到其父对象的children()列表。

当父对象析构的时候，这个列表中的所有对象也会被析构。（注意，这里的父对象并不是继

承意义上的父类！）

我们也可以自己删除子对象，它们会自动从其父对象列表中删除

**建议：从堆区申请空间 而不是从栈区。**

例1，从堆区申请空间:

```C++
//从堆区申请空间
QPushButton *button = new QPushButton(this);
```

例2，从栈区申请空间（quit被释放一次）:

```C++
QWidget window;
//创建一个qiut按钮
QPushButton quit("Quit", &window);
```

入栈顺序：先入window 后quit。释放时，弹栈先调用quit的析构，就会将quit从windos的孩子列表删除，然后windwos调用析构，由于孩子列表中没有对象，就不会再次去释放quit，**qiut只被调用了析构一次**。

例3，从栈区申请空间（quit被释放两次）:

```c++
QPushButton quit("Quit");
QWidget window;
quit.setParent(&window);
```

入栈顺序：先入quit 后window 。释放时，先调用window析构，查看window的孩子列表，调用quit的析构。然后quit出栈，也会调用quit的析构。所以**quit被调用了两次析构**。在 Qt 中，尽量在构造的时候就指定 parent 对象，并且大胆在堆上创建。

# 5 坐标体系

以左上角为原点（0,0），X向右增加，Y向下增加。

对于嵌套窗口，其坐标是**相对于父窗口**来说的。
