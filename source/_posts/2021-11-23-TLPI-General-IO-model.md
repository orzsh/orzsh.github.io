---
title: TLPI：通用的I/O模型
tags: [TLPI, Linux]
categories: [TLPI]
comment: true
date: 2021-11-23 13:12:16
---

Linux下一切皆文件

### 通用I/O

UNIX I/O模型的显著特点之一是其输入/输出的通用性概念。这意味着使用4个同样的系统调用open()、read()、write()、close() 可以对所有类型的文件执行I/O操作，包括终端之类的设备。因此，仅使用这些系统调用编写的程序，将对任何类型的文件有效。

ioctl() 系统调用为通用I/O模型之外的专有特性提供了访问接口。

### 打开一个文件：open()

```C++
#include <sys/stat.h>
#include <fcntl.h>

int open(const char* pathname, int flags, ... /* mode_t mode */);
// Returns file desciptor on success, or -1 on error
```

#### 文件访问模式

- O_RDONLY   以只读方式打开文件
- O_WRONLY   以只写方式打开文件
- O_RDWR   以读写方式打开文件

#### 其他常用flag参数

- O_CLOEXEC   设置 close-on-exec 标志（自 Linux 2.6.23 版本开始）
- O_CREAT   若文件不存在则创建之
- O_EXCL   结合 O_CREAT 参数使用，专门用于创建文件
- O_TRUNC   截断已有文件，使其长度为零
- O_APPEND   总在文件尾部追加数据

#### open()函数的错误

若打开文件时发生错误，open()将返回−1，错误号 errno 标识错误原因。

#### creat()系统调用

```C++
#include <fcntl.h>

int open(const char* pathname, mode_t mode );
//Returns file desciptor, or -1 on error
```

由于 open()的 flags 参数能对文件打 开方式提供更多控制（例如：可以指定 O_RDWR 标志，代替 O_WRONLY 标志），对 creat()的使用 现在已不多见。

### 读取文件内容：read() 

read()系统调用从文件描述符 fd 所指代的打开文件中读取数据。

```C++
#include <unistd.h>

ssize_t read(int fd, void *buffer, size_t count);
//Returns number of bytes read, 0 on EOF, or -1 on error
```

count 参数指定最多能读取的字节数。（size_t 数据类型属于无符号整数类型。）buffer 参数 提供用来存放输入数据的内存缓冲区地址。缓冲区至少应有 count 个字节。

如果 read()调用成功，将返回实际读取的字节数，如果遇到文件结束（EOF）则返回 0， 如果出现错误则返回-1。

### 数据写入文件：write()

write()系统调用将数据写入一个已打开的文件中。

```C++
#include <unistd.h>

ssize_t write(int fd, void *buffer, size_t count);
//Returns number of bytes written, or -1 on error
```

write()调用的参数含义与 read()调用相类似。buffer 参数为要写入文件中数据的内存地址，count 参数为欲从 buffer 写入文件的数据字节数，fd 参数为一文件描述符，指代数据要写入的文件。

如果 write()调用成功，将返回实际写入文件的字节数，该返回值可能小于 count 参数值。

### 关闭文件：close()

close()系统调用关闭一个打开的文件描述符，并将其释放回调用进程，供该进程继续使用。 当一进程终止时，将自动关闭其已打开的所有文件描述符。

```C++
#include <unistd.h>

int close(int fd);
//Returns 0 on success, or -1 on error
```

### 改变文件偏移量：lseek() 

```C++
#include <unistd.h>

off_t lseek(int fd, off_t offset, int whence);     
//Returns new file offset if successful, or -1 error
```

offset 参数指定了一个以字节为单位的数值。（SUSv3 规定 off_t 数据类型为有符号整型 数。）whence 参数则表明应参照哪个基点来解释 offset 参数，应为下列其中之一：

- SEEK_SET  将文件偏移量设置为从文件头部起始点开始的 offset 个字节。 
- SEEK_CUR  相对于当前文件偏移量，将文件偏移量调整 offset 个字节1 。 
- SEEK_END  将文件偏移量设置为起始于文件尾部的 offset 个字节。也就是说，offset 参数应该从文件 最后一个字节之后的下一个字节算起。

图 4-1 展示了 whence 参数的含义。

![img](/assets/TLPI/4-1.jpg)

### 通用 I/O 模型以外的操作：ioctl()

在上述通用 I/O 模型之外，，ioctl()系统调用又为执行文件和设备操作提供了一种多用 途机制。

```C++
#include <sys/ioctl.h>

int ioctl(int fd, int request, ... /* argp */);
//Value returned on success depends on request, or -1 on error
```

fd 参数为某个设备或文件已打开的文件描述符，request 参数指定了将在 fd 上执行的控制操 作。具体设备的头文件定义了可传递给 request 参数的常量。
