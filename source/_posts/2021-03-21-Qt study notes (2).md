---
layout: post
title: Qt学习笔记（二）
date: 2021-03-21 11:53:56
categories: [学习记录]
tag: [Qt, C/C++]

comments: true
---

# 1 信号和槽机机制

信号槽是 Qt 框架引以为豪的机制之一。所谓信号槽，实际就是观察者模式。**当某个事件发生之后**，比如，按钮检测到自己被点击了一下，**它就会发出一个信号（signal）**。这种发出是没有目的的，类似广播。**如果有对象对这个信号感兴趣，它就会使用连接（connect）函数**，意思是，**将想要处理的信号和自己的一个函数（称为槽（slot））绑定来处理这个信号**。也就是说，**当信号发出时，被连接的槽函数会自动被回调**。这就类似观察者模式：当发生了感兴趣的事件，某一个操作就会被自动触发。（这里提一句，**Qt 的信号槽使用了额外的处理来实现，并不是 GoF 经典的观察者模式的实现方式**。）

## 1.1 信号和槽

![01_01](/assets/Qt.assets/01_01.jpg)

项羽宴请刘邦的故事形象的说明了信号和槽的工作方式。



为了体验一下信号槽的使用，我们以一段简单的代码说明：

l  Qt5 的书写方式

```c++
#include <QApplication>
#include <QPushButton>

int main(int argc, char *argv[])

{
    QApplication app(argc, argv);
    QPushButton button("Quit");
    QObject::connect(&button, &QPushButton::clicked, &app, &QApplication::quit);

    button.show();

    return app.exec();
}

```

我们按照前面文章中介绍的在 Qt Creator 中创建工程的方法创建好工程，然后将main()函数修改为上面的代码。点击运行，我们会看到一个按钮，上面有“Quit”字样。点击按钮，程序退出。

connect()函数最常用的一般形式：`connect(sender, signal, receiver, slot);`

参数：

- sender：发出信号的对象
- signal：发送对象发出的信号
- receiver：接收信号的对象
- slot：接收对象在接收到信号之后所需要调用的函数

**信号槽要求信号和槽的参数一致，所谓一致，是参数类型一致。如果不一致，允许的情况是，槽函数的参数可以比信号的少，即便如此，槽函数存在的那些参数的顺序也必须和信号的前面几个一致起来。这是因为，你可以在槽函数中选择忽略信号传来的数据（也就是槽函数的参数比信号的少），但是不能说信号根本没有这个数据，你就要在槽函数中使用（就是槽函数的参数比信号的多，这是不允许的）。**

如果信号槽不符合，或者根本找不到这个信号或者槽函数，比如我们改成：

`connect(&button, &QPushButton::clicked, &QApplication::quit2);`

由于 QApplication 没有 quit2 这样的函数，因此在编译时会有编译错误：

`'quit2' is not a member of QApplication`

这样，使用成员函数指针我们就不会担心在编写信号槽的时候出现函数错误。

l  Qt4 的书写方式：

```C++
int main(int argc, char *argv[]) 
{ 
        QApplication a(argc, argv); 

        QPushButton *button = new QPushButton("Quit"); 

        connect(button, SIGNAL(clicked()), &a, SLOT(quit())); 

        button->show(); 

        return a.exec(); 
}
```

这里使用了**SIGNAL和SLOT这两个宏，将两个函数名转换成了字符串**。注意到connect()函数的 signal 和 slot 都是接受字符串，一旦出现连接不成功的情况，**Qt4是没有编译错误的**（因为一切都是字符串，编译期是不检查字符串是否匹配），而是在运行时给出错误。这无疑会增加程序的不稳定性。

l  Qt5在语法上完全兼容Qt4

## 1.2 自定义信号槽

使用connect()可以让我们连接系统提供的信号和槽。但是，Qt 的信号槽机制并不仅仅是使用系统提供的那部分，还会允许我们自己设计自己的信号和槽。

下面我们看看使用 Qt 的信号槽，实现一个报纸和订阅者的例子：

有一个报纸类Newspaper，有一个订阅者类Subscriber。Subscriber可以订阅Newspaper。这样，当Newspaper有了新的内容的时候，Subscriber可以立即得到通知。

 ////////// newspaper.h //////////

```C++
#include <QObject>
class Newspaper : public QObject

{

    Q_OBJECT

public:

    Newspaper(const QString & name) :

        m_name(name)

    {
    }

    void send()
    {
        emit newPaper(m_name);
    }

signals:

    void newPaper(const QString &name);
private:
    QString m_name;
};
```

////////// reader.h //////////

```C++
#include <QObject>
#include <QDebug>

class Reader : public QObject
{
    Q_OBJECT
    
public:
    Reader() {}
    void receiveNewspaper(const QString & name)
    {
        qDebug() << "Receives Newspaper: " << name;
    }
};

```

////////// main.cpp //////////

```C++
#include <QCoreApplication>

#include "newspaper.h"

#include "reader.h"

int main(int argc, char *argv[])

{

    QCoreApplication app(argc, argv);

    Newspaper newspaper("Newspaper A");

    Reader reader;

    QObject::connect(&newspaper, &Newspaper::newPaper, &reader,    &Reader::receiveNewspaper);

    newspaper.send();

    return app.exec();

}

```

> - 首先看Newspaper这个类。这个类继承了QObject类。**只有继承了QObject类的类，才具有信号槽的能力。**所以，为了使用信号槽，必须继承QObject。**凡是QObject类（不管是直接子类还是间接子类），都应该在第一行代码写上Q_OBJECT**。不管是不是使用信号槽，都应该添加这个宏。这个宏的展开将为我们的类提供信号槽机制、国际化机制以及 Qt 提供的不基于 C++ RTTI 的反射能力。
>
> - Newspaper类的 public 和 private 代码块都比较简单，只不过它新加了一个 signals。signals 块所列出的，就是该类的信号。**信号就是一个个的函数名，返回值是 void（因为无法获得信号的返回值，所以也就无需返回任何值），参数是该类需要让外界知道的数据。信号作为函数名，不需要在 cpp 函数中添加任何实现。**
>
> - Newspaper类的send()函数比较简单，只有一个语句emit newPaper(m_name);。emit 是 Qt 对 C++ 的扩展，是一个关键字（其实也是一个宏）。emit 的含义是发出，也就是发出newPaper()信号。感兴趣的接收者会关注这个信号，可能还需要知道是哪份报纸发出的信号？所以，我们将实际的报纸名字m_name当做参数传给这个信号。当接收者连接这个信号时，就可以通过槽函数获得实际值。这样就完成了数据从发出者到接收者的一个转移。
>
> - Reader类更简单。因为这个类需要接受信号，所以我们将其继承了QObject，并且添加了Q_OBJECT宏。后面则是默认构造函数和一个普通的成员函数。**Qt 5 中，任何成员函数、static 函数、全局函数和 Lambda 表达式都可以作为槽函数。**与信号函数不同，槽函数必须自己完成实现代码。槽函数就是普通的成员函数，因此作为成员函数，也会受到 public、private 等访问控制符的影响。（如果信号是 private 的，这个信号就不能在类的外面连接，也就没有任何意义。）

### **自定义信号槽需要注意的事项**

> - **发送者和接收者都需要是QObject的子类（当然，槽函数是全局函数、Lambda 表达式等无需接收者的时候除外）；**
>
> - **使用 signals 标记信号函数，信号是一个函数声明，返回 void，不需要实现函数代码；**
>
> - **槽函数是普通的成员函数，作为成员函数，会受到 public、private、protected 的影响；**
>
> - **使用 emit 在恰当的位置发送信号；**
>
> - **使用QObject::connect()函数连接信号和槽。**
>
> - **任何成员函数、static 函数、全局函数和 Lambda 表达式都可以作为槽函数**

### **信号槽的更多用法**

> - 一个信号可以和多个槽相连
>
>   **如果是这种情况，这些槽会一个接一个的被调用，但是它们的**调用顺序是不确定的。
>
> - 多个信号可以连接到一个槽
>
>   **只要任意一个信号发出，这个槽就会被调用**。
>
> - 一个信号可以连接到另外的一个信号
>
>   **当第一个信号发出时，第二个信号被发出。除此之外，这种信号-信号的形式和信号-槽的形式没有什么区别。**
>
> - 槽可以被取消链接
>
>   这种情况并不经常出现，因为**当一个对象delete之后，Qt自动取消所有连接到这个对象上面的槽**。
>
> - 使用Lambda 表达式
>
>   在使用 Qt 5 的时候，能够支持 Qt 5 的编译器都是支持 Lambda 表达式的。
>
>   我们的代码可以写成下面这样：
>
>   ```C++
>   QObject::connect(&newspaper, static_cast<void (Newspaper:: *)
>   
>   (const QString &)>(&Newspaper::newPaper),=](const QString &name) 
>   
>   { /* Your code here. */ });
>   ```
>
>   在连接信号和槽的时候，槽函数可以使用Lambda表达式的方式进行处理。

### 总结：

>- 一个信号可以和多个槽相连，如果是这种情况，这些槽会一个接一个的被调用，但是它们的调用顺序是不确定的。
>
>- 多个信号可以连接到一个槽，只要任意一个信号发出，这个槽就会被调用。
>
>- 一个信号可以连接到另外的一个信号，当第一个信号发出时，第二个信号被发出。除此之外，这种信号-信号的形式和信号-槽的形式没有什么区别。
>
>- 槽可以被取消链接,这种情况并不经常出现，因为当一个对象delete之后，Qt自动取消所有连接到这个对象上面的槽。
>
>- Qt4版本的信号槽写法
>
>  ```C++
>  connect(tea,SIGNAL(hungry()), stu, SLOT(treat()))
>  ```
>
>- Qt5版本的信号槽写法
>
>  ```C++
>  connect(tea, &Teacher::hungry, stu, &Student::treat)
>  ```

  