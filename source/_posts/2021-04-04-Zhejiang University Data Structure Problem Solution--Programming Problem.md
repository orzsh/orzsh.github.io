---
layout: post
title: 浙江大学数据结构2021春--编程题
date: 2021-04-04 12:34:54
categories: [学习记录]
tag: [数据结构]

comments: true
---

# 01-复杂度1 最大子列和问题 (20 分)

给定*K*个整数组成的序列{ *N*1, *N*2, ..., *N**K* }，“连续子列”被定义为{ *N**i*, *N**i*+1, ..., *N**j* }，其中 1≤*i*≤*j*≤*K*。“最大子列和”则被定义为所有连续子列元素的和中最大者。例如给定序列{ -2, 11, -4, 13, -5, -2 }，其连续子列{ 11, -4, 13 }有最大的和20。现要求你编写程序，计算给定整数序列的最大子列和。

本题旨在测试各种不同的算法在各种数据情况下的表现。各组测试数据特点如下：

- 数据1：与样例等价，测试基本正确性；
- 数据2：102个随机整数；
- 数据3：103个随机整数；
- 数据4：104个随机整数；
- 数据5：105个随机整数；

**输入格式**:

输入第1行给出正整数*K* (≤100000)；第2行给出*K*个整数，其间以空格分隔。

**输出格式**:

在一行中输出最大子列和。如果序列中所有整数皆为负数，则输出0。

**输入样例:**

```in
6
-2 11 -4 13 -5 -2
```

**输出样例**:

```out
20
```

**解法一(暴力)：**

```C++
/*思路：运用两层循环，第一层循环用来确定子列的第一个元素，第二层循环用来遍历每个已经确定了第一个元素的子列
*/
#include<iostream>
using namespace std;
int main(){
    int N, ThisSum, MaxSum=0;
    cin >> N;
    int arr[N];
    for(int i=0;i<N;i++){
        cin >> arr[i];
    }
    for(int i=0;i<N;i++){ //子列左端位置
        ThisSum = 0;
        for(int j=i;j<N;j++){ //子列右端位置
            ThisSum += arr[j];
            if(ThisSum > MaxSum)
                MaxSum = ThisSum;
        }
    }
    cout << MaxSum;
    return 0;
}
```

**解法二（递归）：**

```C++
//这是一种分而治之的思想
int Max3( int A, int B, int C )
{ /* 返回3个整数中的最大值 */
    return A > B ? A > C ? A : C : B > C ? B : C;
}

int DivideAndConquer( int List[], int left, int right )
{ /* 分治法求List[left]到List[right]的最大子列和 */
    int MaxLeftSum, MaxRightSum; /* 存放左右子问题的解 */
    int MaxLeftBorderSum, MaxRightBorderSum; /*存放跨分界线的结果*/

    int LeftBorderSum, RightBorderSum;
    int center, i;

    if( left == right )  { /* 递归的终止条件，子列只有1个数字 */
        if( List[left] > 0 )  return List[left];
        else return 0;
    }

    /* 下面是"分"的过程 */
    center = ( left + right ) / 2; /* 找到中分点 */
    /* 递归求得两边子列的最大和 */
    MaxLeftSum = DivideAndConquer( List, left, center );
    MaxRightSum = DivideAndConquer( List, center+1, right );

    /* 下面求跨分界线的最大子列和 */
    MaxLeftBorderSum = 0; LeftBorderSum = 0;
    for( i=center; i>=left; i-- ) { /* 从中线向左扫描 */
        LeftBorderSum += List[i];
        if( LeftBorderSum > MaxLeftBorderSum )
            MaxLeftBorderSum = LeftBorderSum;
    } /* 左边扫描结束 */

    MaxRightBorderSum = 0; RightBorderSum = 0;
    for( i=center+1; i<=right; i++ ) { /* 从中线向右扫描 */
        RightBorderSum += List[i];
        if( RightBorderSum > MaxRightBorderSum )
            MaxRightBorderSum = RightBorderSum;
    } /* 右边扫描结束 */

    /* 下面返回"治"的结果 */
    return Max3( MaxLeftSum, MaxRightSum, MaxLeftBorderSum + MaxRightBorderSum );
}

int MaxSubseqSum3( int List[], int N )
{ /* 保持与前2种算法相同的函数接口 */
    return DivideAndConquer( List, 0, N-1 );
}
```

**解法三（在线处理）：**

```C++
int MaxSubseqSum(int A[], int N){
    int ThisSum=0, MaxSum = 0;
    int i;
    for(int i=0;i<N;i++){
        ThisSum += A[i]; //向右累加
        if(ThisSum > MaxSum)
            MaxSum = ThisSum; //发现更大和则更新当前结果
    	else if(ThisSum < 0) //如果当前子列和为负
            ThisSum = 0;//则不能使后面结果增大，抛弃之
    }
    return MaxSum;
}
```



# 01-复杂度2 Maximum Subsequence Sum (25 分)

Given a sequence of *K* integers { *N*1, *N*2, ..., *N**K* }. A continuous subsequence is defined to be { *N**i*, *N**i*+1, ..., *N**j* } where 1≤*i*≤*j*≤*K*. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

**Input Specification:**

Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer *K* (≤10000). The second line contains *K* numbers, separated by a space.

**Output Specification:**

For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices *i* and *j* (as shown by the sample case). If all the *K* numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

**Sample Input:**

```in
10
-10 1 2 3 4 -5 -23 3 7 -21
```

**Sample Output:**

```out
10 1 4
```

**题解：**

由于这题没看清题目（英语蒟蒻），导致改了半天也没AC，都快WA到吐了，就上CSDN搜了一下，原来要输出的是最大子列的首尾对应元素而不是下标。

```C++
#include <iostream>

using namespace std;
int main(){
    int k;
    cin >> k;
    int arr[k];
    int ThisSum = 0, MaxSum = -1;
    int start=0, end=0, tmp=0; 
    for(int i=0;i<k;i++){
        cin >> arr[i];
        ThisSum += arr[i];
        if(ThisSum > MaxSum){
            MaxSum = ThisSum;
            start = tmp;
            end = i; //记录ThisSum的最后一项
        }
        else if(ThisSum < 0){
            ThisSum = 0; //重置现在的子列和
            tmp = i+1; //暂时记录下一次将要计算的子列和的首项
        }
    }
    if(MaxSum<0) printf("0 %d %d", arr[0], arr[k-1]);
    else printf("%d %d %d",MaxSum, arr[start], arr[end]);
    return 0;
}
```



未完待续...