---
layout: post
title: 浙江大学数据结构2021春--函数题
date: 2021-04-04 11:11:08
categories: [学习记录]
tag: [数据结构]
---

# 01-复杂度3 二分查找 (20 分)

本题要求实现二分查找算法。

**函数接口定义：**

```c++
Position BinarySearch( List L, ElementType X );
```

其中`List`结构定义如下：

```c++
typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};
```

`L`是用户传入的一个线性表，其中`ElementType`元素可以通过>、==、<进行比较，并且题目保证传入的数据是递增有序的。函数`BinarySearch`要查找`X`在`Data`中的位置，即数组下标（注意：元素从下标1开始存储）。找到则返回下标，否则返回一个特殊的失败标记`NotFound`。

**裁判测试程序样例：**

```c++
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 10
#define NotFound 0
typedef int ElementType;

typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};

List ReadInput(); /* 裁判实现，细节不表。元素从下标1开始存储 */
Position BinarySearch( List L, ElementType X );

int main()
{
    List L;
    ElementType X;
    Position P;

    L = ReadInput();
    scanf("%d", &X);
    P = BinarySearch( L, X );
    printf("%d\n", P);

    return 0;
}

/* 你的代码将被嵌在这里 */
```

**输入样例1：**

```in
5
12 31 55 89 101
31
```

**输出样例1：**

```out
2
```

**输入样例2：**

```
3
26 78 233
31
```

**输出样例2：**

```
0
```

**题解：**

```C++
/*思路：先排好序，每次从中间查找，如果待查元素比中间元素小，则待查元素在中间元素左边，更新右边界；如果待查元素比中间元素大，则待查元素在中间元素右边，更新左边界。
*/
Position BinarySearch( List L, ElementType X ){
	int left, mid, right;
    left = 1;
    right = L->Last;
    while(left<=right){ //左边界不大于右边界
        mid = (left+right)/2; //更新中间值的位置
        if(X<L->Data[mid]) //待查元素小于中间元素的值则更新右边界
            right = right-1;
        else if(X>L->Data[mid]) //待查元素小于中间元素的值则更新左边界
            left = mid+1;
        else //其他情况即找到了，则返回位置mid
            return mid;
    }
	return NotFound;
}
```



# 02-线性结构1 两个有序链表序列的合并 (15 分)

本题要求实现一个函数，将两个链表表示的递增整数序列合并为一个非递减的整数序列。

**函数接口定义：**

```c++
List Merge( List L1, List L2 );
```

其中`List`结构定义如下：

```c++
typedef struct Node *PtrToNode;
struct Node {
    ElementType Data; /* 存储结点数据 */
    PtrToNode   Next; /* 指向下一个结点的指针 */
};
typedef PtrToNode List; /* 定义单链表类型 */
```

`L1`和`L2`是给定的带头结点的单链表，其结点存储的数据是递增有序的；函数`Merge`要将`L1`和`L2`合并为一个非递减的整数序列。应直接使用原序列中的结点，返回归并后的带头结点的链表头指针。

**裁判测试程序样例：**

```c++
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;
typedef struct Node *PtrToNode;
struct Node {
    ElementType Data;
    PtrToNode   Next;
};
typedef PtrToNode List;

List Read(); /* 细节在此不表 */
void Print( List L ); /* 细节在此不表；空链表将输出NULL */

List Merge( List L1, List L2 );

int main()
{
    List L1, L2, L;
    L1 = Read();
    L2 = Read();
    L = Merge(L1, L2);
    Print(L);
    Print(L1);
    Print(L2);
    return 0;
}

/* 你的代码将被嵌在这里 */
```

**输入样例：**

```in
3
1 3 5
5
2 4 6 8 10
```

**输出样例：**

```out
1 2 3 4 5 6 8 10 
NULL
NULL
```

**题解：**

```c
List Merge( List L1, List L2 ){
    List pHead = (List)malloc(sizeof(struct Node)); //创建一个头节点准备接收合并后的链表
    pHead->Next = NULL;
    List p1 = L1->Next; //带头节点的链表从头节点下一个位置开始存储数据
    List p2 = L2->Next; //获取L1,L2的第一个有数据的节点
    List p3 = pHead; //创建p1,p2,p3来分别操作L1，L2，pHead(合并后的新链表)是在他们上面移动，并不改变他们本身
    while(p1&&p2){ //当两个节点都不为空时一直循环
        if(p1->Data < p2->Data){ //若p1指向的节点数据的值小于p2的则让p1连接到p3后面，即谁小把谁连接到p3后面
            p3->Next = p1; //把p1连到p3后面
            p3 = p1; //更新p3指向的位置，以便让下一个节点连接
            p1 = p1->Next; //p1指向L1的下一个节点
        }
        else{
            p3->Next = p2;
            p3 = p2;
            p2 = p2->Next;
        }
    }
    if(p1) p3->Next = p1; //如果p1不为空，则还有节点未连接，将剩下的节点连接到p3后面
    if(p2) p3->Next = p2; //如果p2不为空，则还有节点未连接，将剩下的节点连接到p3后面
    L1->Next = NULL; //L1指空
    L2->Next = NULL; //L2指空
    return pHead; //返回合并后的新链表的头节点
}
```

未完待续...