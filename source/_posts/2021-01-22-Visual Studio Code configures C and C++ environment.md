---
layout: post
title: Visual Studio Code配置C/C++环境（踩坑记录）
date: 2021-01-22 08:20:47
categories: [学习记录]
tags: [踩坑记录, VS Code, C/C++]
comments: true
---

#  前言

昨晚正用着Dev-C++，突然就来了个想法——这玩意儿怎么能这么丑（可怜的Dev-C++.jpg）。这时我就想起了的VS Code，很早就听说它界面设计简洁大方，主题配色赏心悦目，布局合理，体验流畅，不过最重要的还是它免费啊（~~是白嫖的诱惑，大声~~）。是骡子是马拿出来遛遛知道了，我的踩坑之路就开始了。

# 基本流程

1、安装VS Code（下载链接：https://code.visualstudio.com/Download）
2、安装配置环境，网上大多都推荐MinGW-w64（下载链接：https://sourceforge.net/projects/mingw-w64/files）
3、配置下列几个文件：
	launch.json，tasks.json 
4、完成配置，嗨起来

# 踩坑记录

## 下载安装

#### 1.VS Code

先去官网找到适合自己的VS Code安装方法，点击下载，这时候问题就来了，是不是觉得下载的很慢？

![](/assets/blogImg/VScode1.jpg)

下载速度只有几十KB/s，没耐心的同学肯定是受不了的。这时候我打开了科学上网，速度马上就增加到了1MB/s，没有科学上网的同学只好等啦。

安装的时候一直点下一步就行了。不过记得检查是否勾选添加到PATH，没有的勾的记得勾上。

![](/assets/blogImg/VScode2.jpg)

### 2.MinGW

这个没什么难度，而且网上教程一大堆，这里就不作赘述了（~~真的不是想偷懒~~）。

## 配置文件

这里就是浪费我大量时间的地方了。

### 1.创建文件夹

随便找个你喜欢的路径建一个文件夹，**注意：路径内一定不能出现中文**，我开始就是建了一个中文的文件夹，导致一直调试不成功，时不时佩服一下自己的鬼才操作啊。

我的路径就是下面这个样子：

![](/assets/blogImg/VScode3.jpg)

### 2.创建C++文件

在你新建的文件夹下新建个C++文件，暂且命名为test.cpp吧。然后右键点击它，找到打开方式选择VS Code。然后我们随便输入下面这段代码后按下F5。

```c++
#include <iostream>
using namespace std;

int main()
{
    cout << "Hello World!";
    cin.get();  // cin.get()是为了使命令行窗口停住  
    return 0;
}
```

按下之后选择C++(GDB//LLDB)

![](/assets/blogImg/VScode4.jpg)

再选择g++.exe

![](/assets/blogImg/VScode5.jpg)

之后会生成两个配置文件：launch.json，tasks.json，下面我们来配置它们。

### 3.配置launch.json

```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "g++.exe - 生成和调试活动文件",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "D:\\MingGW64\\mingw64\\bin",
            "environment": [],
            "externalConsole": true, 	// 修改此处
            "MIMode": "gdb",
            "miDebuggerPath": "D:\\MingGW64\\mingw64\\bin\\gdb.exe",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "g++" // 修改此处，这里应该与tasks.json的label内容一致
        }
    ]
}
```

### 4.配置tasks.json

```json
{
    "tasks": [
        {
            "type": "cppbuild",
            "label": "g++",  // 修改此处，切记这里应该与launch.json的preLaunchTask内容一致
            "command": "D:\\MingGW64\\mingw64\\bin\\g++.exe",
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "D:\\MingGW64\\mingw64\\bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "detail": "调试器生成的任务。"
        }
    ],
    "version": "2.0.0"
}
```

### 5.最后测试

回到VS Code，找到test.cpp，按下F5，出现下面这样就说明你成功啦，开始happy~吧。

![](/assets/blogImg/VScode6.jpg)