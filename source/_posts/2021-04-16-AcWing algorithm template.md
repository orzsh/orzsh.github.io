---
layout: post
title: AcWing算法基础课模板
date: 2021-04-16 23:27:07
categories: [学习记录]
tag: [AcWing, 算法, 刷题]

comments: true
---

# 一、基础算法

包括排序、二分、高精度、前缀和与差分、双指针算法、位运算、离散化、区间合并等内容。

## Quick_sort

①确定分界点，取左边界q[l]或右边界q[r]或中间点q[(l+r)/2]或随机一点。

②调整区间，x（分界点）左边的数都小于x，右边的数都大于x。（**核心步骤**）

③递归处理左右两段。

平均时间复杂度：nlogn

最坏时间复杂度：n^2

例题：[785. 快速排序](https://www.acwing.com/problem/content/787/) 、[786. 第k个数](https://www.acwing.com/problem/content/788/)

```C++
void quick_sort(int q[], int l, int r){
    if(l>=r) return; //区间内只有1个或者没有元素直接返回
    
    //i为左指针，j为右指针，注意边界
    int i = l-1, j = r+1, x = q[l+r>>1]; //l+r的值右移1位，相当l+r的值除以2取整。
    while(i<j){ //左指针小于右指针
        do i++; while(q[i] < x); //指针i右移，x左边的数小于x就循环
        do j--; while(q[j] > x); //指针j左移，x右边的数大于x就循环
        if(i < j) swap(q[i], q[j]); //如果左右指针没有相遇，就交换所指元素的位置
    }
    //递归处理左右两段
    quick_sort(q, l, j)，quick_sort(q, j+1, r);
}
```

## 归并排序——分而治之

①确定分界点：mid =(l+r)/2

②递归排序左半边和右半边

③归并--合二为一（**核心步骤**）

平均时间复杂度：nlogn

例题：[787. 归并排序](https://www.acwing.com/problem/content/789/)、[788. 逆序对的数量](https://www.acwing.com/problem/content/790/)

```C++
void merge_sort(int p[], l, r){
    if(l>=r) return; //区间内只有1个或者没有元素直接返回
    
    int mid = l+r>>1;
    merge_sort(p, l, mid);
    merge_sort(p, mid+1, r);
    
    int tmp[N]; //开辟一个临时数组保存结果，N可以依据题目定义在函数体外
    int k = 0, i = 1, j = mid + 1; //k表示当前tmp里面已经存了多少个数
    while(i <= mid && j <= r){
        if (q[i] <= q[j]) tmp[k++] = q[i++]; //把小的存入tmp中（从小到大排）
        else tmp[k++] = q[j++];
    }
    while(i <= mid) tmp[k++] = q[i++]; //左半边有剩余就把剩余的连上去
    while(j <= r) tmp[k++] = q[j++];//右半边有剩余就把剩余的连上去
    
    for(i = l, j=0;i <= r; i++, j++) q[i] = tmp[j]; //最后将tmp复制回q
}
```

## 整数二分

例题：[789. 数的范围](https://www.acwing.com/problem/content/791/)

```C++
bool check(int x) {/* ...*/} //检查x是否满足某种性质
//模板1：
//区间[l, r]被划分成[l, mid]和[mid+1, r]时使用：
int bsearch_1(int l, int r){
    while(l < r){
    int mid = l+r>>1;
    if(check(mid)) r = mid; //check()判断mid是否满足性质
    else l = mid + 1;
    }
    return l;
}
//模板2：
//区间[l, r]被划分成[l, mid-1]和[mid, r]时使用：
int bsearch_2(int l,int r){
    while(l<r){
        int mid = l + r + 1 >> 1;
        if(check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```



## 高精度加法

例题: [791. 高精度加法](https://www.acwing.com/problem/content/793/) 

```C++
// C = A + B, A >= 0, B >= 0
//传入A、B前先将string转成vector,且数是以低位在前高位在后存储在A,B中的，这样方便进位。
vector<int> add(vector<int> &A, vecotr<int> &B){
	if(A.size() < B.size()) return add(B, A);
    
    vector<int> C;
    int t = 0; //临时变量，保存进位，初始进位为0
    for(int i=0;i<A.size();i++){
        t += A[i];
        if(i<B.size()) t += B[i]; //如果B中还有数字
        C.push_back(t%10);
        t /= 10;
    }
    if(t) C.push_back(t); //如果最后一位还有进位就push_back
    return C;
}
```

## 高精度减法

例题：[792.高精度减法](https://www.acwing.com/problem/content/794/)

```C++
// C = A - B，满足 A >= B, A >=0，B >= 0
vector<int> sub(vector<int> &A,vecotr<int> &B){
    vector<int> C;
    for(int i = 0, t = 0;i < A.size(); i++){
        t = A[i] - t; //看看上一位有没借位
        if(i<B.size()) t -= B[i]; //如果此时B中还有数,就减去这位数
        C.push_back((t+10)%10); //有t>=0和t<0两种情况，①t>=0: t = t ②t<0: t = t+10（加10相当于借了一位）
        if(t<0) t = 1; //减完之后如果t<0，说明发生了借位，t=1 就能让下一次循环的开始减去1
      	else t = 0;
    }
    while(C.size()>1 && C.back()==0) c.pop_back();
    return C;
}
```

## 高精度乘低精度

例题：[793. 高精度乘法](https://www.acwing.com/problem/content/795/)

```C++
// c = A * B, A >= o, b >= 0
vector<int> mul(vector<int> &A, int b){
    vector<int> C;
    
    int t = 0; //进位
    for(int i=0;i<A.size()||t;i++){ //t不为0时表示还有进位就要继续循环
        if(i<A.size()) t += A[i] * b;
        C.push_back(t%10);
        t /= 10; //取进位
    }
    
    while(C.size() > 1 && C.back() == 0) C.pop_back(); //去除前导0，只剩一位时就不用去除
    return C;
    
    return C；
}
```

## 高精度除以低精度

例题：[794. 高精度除法](https://www.acwing.com/problem/content/796/)

```C+++
// C = A / b ... r , A >= 0 , b > 0
vector<int> div (vector<int> &A, vecotr<int> &b, r){
	vector<int> C;
	r = 0;
	for(int i= i.size() - 1; i >= 0; i--){
		r = r * 10 + A[i];
		C.push_back(r/b);
		r %= b;
	}
	reverse(C.begin(), C.end());
	while(C.size() > 1 && C.back() == 0) C.pop_back(); //去除前导0
	return C；
}

```

## 一维前缀和

例题：[795. 前缀和](https://www.acwing.com/problem/content/797/)

```C++
s[i] = a[1] + a[2] + ... + a[i]
a[l] + ... a[r] = s[r] - s[l - 1]
```

## 二维前缀和

例题：[796. 子矩阵的和](https://www.acwing.com/problem/content/798/)

```C++
s[i, j] = 第i行j列格子左上部分所有元素的和
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
s[x2, y2] - s[x1 - 1, y2] - s[x2, y1-1] + s[x1 -1, y1 - 1]
```

## 一维差分

例题：[797. 差分](https://www.acwing.com/problem/content/799/)

```C++
给区间[l ,r]中的每个数加上c：B[l] += c, B[r + 1] += c
```

## 二维差分

例题：[798. 差分矩阵](https://www.acwing.com/problem/content/800/)

```C++
给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中所有的元素加上C：
s[x1, y1] += c, s[x2 + 1, y1] -= c, s[x1, y2 + 1] -= c, s[x2 + 1, y2 + 1] += c  
```

## 位运算

例题：[801. 二进制中1的个数](https://www.acwing.com/problem/content/803/)

```C++
求n的第k位数字：n >> k & 1
返回n的最后一位1：lowbit(n) = n & -n
```

