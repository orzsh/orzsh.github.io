---
layout: post
title: 顺序表及其操作
date: 2021-1-26 12:12:43
categories: [学习记录]
tag: [数据结构]

comments: true
---

# 顺序表的基本操作

InitList(&L) ：初始化表。构造一个空的线性表L，分配内存空间。

DestroyList(&L,i,e)：销毁操作。销毁线性表，并释放线性表L所占用的内存空间。

ListInsert(&L,i,e)：插入操作。在表L中的第i个位置上插入指定元素e。

ListDetele(&L,i,&e)：删除操作。删除表L中第i个位置的元素，并用e返回删除元素的值。

LocateElem(L,e)：按值查找操作。在表L中查找具有给定关键词的元素。

GetElem(L,i)：按位查找操作。获取表L中第i个位置的元素。

## 方法1：静态分配

### 创建

```	c++
#define MaxSize 10	//定义最大长度
typedef struct{
	int data[MxaSize];	//用静态的“数组”存放数据元素
    int length;	//顺序表的类型定义（静态分配方式）
}SqList;
```

### 初始化

```C++
void InitList(SqList &L){
	for(int i=0; i<MaxSize; i++)
        L.data[i]=0;
    L.length=0;
}
```

## 方法2：动态分配

### 创建

```C++
#define InitSize 10	//顺序表的初始长度
typedef struct{
    int *data;	//指示动态分配数组的指针
    int MxaSize;	//顺序表当前长度
    int length;	//顺序表的当前长度
}SeqList;	//顺序表的类型定义（动态分配方式）
```

### 初始化

```C++
void InitList(SeqList &L){
    //用malloc()申请一片连续的存储空间
    L.data=(int*)malloc(InitSize*sizeof(int));
    L.MaxSize = InitSize;
    L.length=0;
}
```

### 增加长度

```C++
void IncreaseSize(SeqList &L, int len){
    int *p = L.data;
    L.data = (int*)malloc((L.MaxSize+len)*sizeof(int));
    for(int i=0; i<L.length; i++){
        L.data[i] = p.data[i];
    }
    L.MaxSize = L.MaxSize+len;
    free(p);
}
```

## 增删查改

### 插入

```C++
void ListInsert(SqList &L,int i,int e){	//在位置i处插入元素e
    if(i<1||i>L.length+1) //判断i的范围是否有效
        return false;
    if(L.length>=MaxSize)	//当前存储空间已满，不能插入
        return false;
    for(int j=L.length;j>=i;j--)	//将第i个元素及之后的元素后移
        L.data[j]=L,data[j-1];
    L.data[i-1]=e;	//在位置i处放入e
    L.length++;	//长度加1
}
```

### 删除

```C++
//删除第i个位置的元素再用e返回该元素的值
void ListDelete(&L,i,&e){
    if(i<1||i>L.length) //判断i的范围是否有效
        return flase;
    e = L.length[i-1];	//将第i个元素赋值给e
    for(int j=i;j<L.length;j++)	//将第i个位置之后的元素往前移
        L.data[j-1]=L.data[j];
    L.length--; //表长减1
}
```

### 按位查找

```C++
int GetElem(SqList L,int i){
    return L.data[i-1];
}
```

### 按值查找

```C++
//在顺序表L中查找第一个元素值等于e的元素，并返回其位序
int LocateElem(SqList L,int e){
	for(int i=0;i<L.length;i++)
        if(L.data[i]==e)
            return i+1; //数组下标为1的元素值等于e，返回其位序i+1
    return 0;	//退出循环
}
```

