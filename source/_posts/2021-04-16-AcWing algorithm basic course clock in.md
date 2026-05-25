---
layout: post
title: AcWing算法基础课打卡
date: 2021-04-16 17:17:07
categories: [学习记录]
tag: [AcWing, 算法]
comments: true
---

## [785. 快速排序](https://www.acwing.com/problem/content/787/)

```C++
#include <iostream>

using namespace std;
const int N=1e6+10;
int q[N];

void quick_sort(int q[], int l, int r){
    if(l>=r) return;
    int i=l-1, j=r+1, x=q[l+r >> 1];
    while(i<j){
        do i++;while(q[i] < x);
        do j--;while(q[j] > x);
        if(i<j) swap(q[i], q[j]);
    }
    
    quick_sort(q, l, j),quick_sort(q, j+1, r);
}

int main(){
    int n;
    scanf("%d", &n);
    for(int i=0;i<n;i++) scanf("%d", &q[i]);
    
    quick_sort(q, 0, n-1);
    for(int i=0;i<n;i++) printf("%d ", q[i]);
    return 0;
}
```

## [786.第k个数](https://www.acwing.com/problem/content/788/)

```C++
#include<iostream>
using namespace std;

void quick_sort(int q[], int l, int r){
    if (l>=r) return; //只有一个或者没有元素返回
    int i = l-1, j = r+1, x = q[l+r>>1];
    while(i<j){
        do i++;while(q[i] < x);
        do j--;while(q[j] > x);
        if(i<j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j);
    quick_sort(q, j+1, r);
}

const int N=1e6+10;
int n,k;
int q[N];
int main(){
    cin >> n >> k;
    for(int i=0;i<n;i++) scanf("%d" ,&q[i]);
    
    quick_sort(q, 0, n-1);
    
    printf("%d", q[k-1]);
    
    return 0;
}
```

## [787. 归并排序](https://www.acwing.com/problem/content/789/)

```C++
#include<iostream>
using namespace std;

const int N = 1e6+10;

void merge_sort(int q[],int l,int r){
    if(l>=r) return;
    int mid = l+r>>1; //确定中点

    merge_sort(q, l, mid);
    merge_sort(q, mid+1, r); 
    
    int tmp[N];//开辟一个临时数组保存结果
    int k = 0, i = l, j = mid+1; 
    while(i<=mid && j<=r){
        if(q[i] <= q[j]) tmp[k++]=q[i++];
        else tmp[k++] = q[j++];
    }
    while(i<=mid) tmp[k++] = q[i++];
    while(j<=r) tmp[k++] = q[j++];
    
    for(int i=l,j=0;i<=r;i++,j++) q[i] = tmp[j];
}

int main(){
    int n, q[N];

    scanf("%d", &n);
    
    for(int i=0;i<n;i++) scanf("%d", &q[i]);
    
    merge_sort(q, 0, n-1);
    
    for(int i=0;i<n;i++) printf("%d ", q[i]);
    
    return 0;
}
```

## [788. 逆序对的数量](https://www.acwing.com/problem/content/790/)

```C++
#include<iostream>

using namespace std;

const int N = 1e6+10;
int q[N], tmp[N];
typedef long long LL; //最大可能会有1e10个

LL merge_sort(int l, int r){
    if(l>=r) return 0;
    
    int mid = l+r>>1;    
    LL res = merge_sort(l, mid) + merge_sort(mid+1, r);
    
    int k=0, i=l, j = mid+1;
    while(i<=mid && j<=r){
        if(q[i] <= q[j]) tmp[k++] = q[i++];
        else{
            tmp[k++] = q[j++];
            res += mid-i+1; //记录逆序数个数
        }
    }
    //扫尾
    while(i <= mid) tmp[k++] = q[i++];
    while(j <= r) tmp[k++] = q[j++];
    
    for(int i=l,j=0;i<=r;i++,j++) q[i] = tmp[j];
    return res;
}

int main(){
    int n;
    scanf("%d", &n);
    for(int i=0;i < n; i++)
        scanf("%d", &q[i]); 
    cout << merge_sort(0, n-1) << endl;;
    
    return 0;
}
```

## [789. 数的范围](https://www.acwing.com/problem/content/791/)

```C++
#include <iostream>

using namespace std;

const int N = 100010;

int n, m;
int q[N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);

    while (m -- )
    {
        int x;
        scanf("%d", &x);

        int l = 0, r = n - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (q[mid] >= x) r = mid;
            else l = mid + 1;
        }

        if (q[l] != x) cout << "-1 -1" << endl;
        else
        {
            cout << l << ' ';

            int l = 0, r = n - 1;
            while (l < r)
            {
                int mid = l + r + 1 >> 1;
                if (q[mid] <= x) l = mid;
                else r = mid - 1;
            }

            cout << l << endl;
        }
    }

    return 0;
}

```

## [790. 数的三次方根](https://www.acwing.com/problem/content/792/)

```C++
#include <iostream>
using namespace std;

int main(){
    double n;
    cin >> n;
    double l=-10000, r=10000;
    
    while((r-l) > 1e-8){ //1e-8是经验值
        double mid = (l+r)/2;
        if(mid*mid*mid>=n) r = mid;
        else l = mid;
    }
    printf("%lf", l);
    return 0;
}
```

## [791. 高精度加法](https://www.acwing.com/problem/content/793/)

```C++
#include<iostream>
#include <vector>

using namespace std;
const int N = 100010;

vector<int> add(vector<int> &A, vector<int> &B){
    if(A.size() < B.size()) return add(B,A);
    
    vector<int> C;
    int t = 0;//表示进位
    for(int i=0;i<A.size();i++){
        t += A[i];
        if(i < B.size()) t += B[i];
        C.push_back(t%10);
        t /= 10;
    }
    
    if(t) C.push_back(t); //当A、B一样长时，缺少此条件就会WA
    return C;
}


int main(){
    string a,b;
    cin >> a >> b;
    
    vector<int> A,B;
    for(int i=a.size()-1;i>=0;i--) A.push_back(a[i] - '0');
    for(int i=b.size()-1;i>=0;i--) B.push_back(b[i] - '0');
    
    vector<int> C = add(A,B);
    
    for(int i=C.size() - 1; i>=0; i--) cout << C[i];
    cout << endl;
    
    return 0;
}
```

## [792. 高精度减法](https://www.acwing.com/problem/content/794/)

```C++
#include <iostream>
#include <vector>

using namespace std;
bool cmp(vector<int> &A, vector<int> &B){
    if(A.size() != B.size()) return A.size() > B.size();
    
    //A,B一样长时
    for(int i=A.size() - 1;i >= 0;i--){
        if(A[i] != B[i])
            return A[i] > B[i];
    }
    
    return true;
}

vector<int> sub(vector<int> &A, vector<int> &B){
    vector<int> C;
    int t = 0; //表示借位
    for(int i=0;i < A.size(); i++){
        t = A[i] - t;
        if(i < B.size()) t -= B[i];
        C.push_back((t+10)%10); //当t为负数时就要借位，当t为正时不影响结果，两种情况合并着写
        if(t<0) t = 1; //发生借位
        else t=0;
    }
    while(C.size() > 1 && C.back()==0) C.pop_back(); //去除前导0
    return C;
}


int main(){
    string a,b;
    cin >> a >> b;
    
    vector<int> A, B;
    for(int i=a.size()-1;i>=0;i--) A.push_back(a[i] - '0');
    for(int i=b.size()-1;i>=0;i--) B.push_back(b[i] - '0');
    
    vector<int> C;
    if(cmp(A,B)) C = sub(A,B); //比较A,B的大小
    else C = sub(B,A), cout << '-';

    for(int i=C.size()-1;i>=0; i--)
        cout << C[i];
    cout << endl;
    
    return 0;
}
```

## [793. 高精度乘法](https://www.acwing.com/problem/content/description/795/)

```C++
#include<iostream>
#include<vector>

using namespace std;

vector<int> mul(vector<int> &A, int b){
    vector<int> C;
    
    int t = 0; //保存进位,t可能不止一位
    for(int i = 0; i< A.size() || (t!=0) ;i++){ 
        if(i<A.size()) t += A[i] * b;
        C.push_back(t % 10); //保存个位
        t /= 10; //进位
    }
    
    while(C.size() > 1 && C.back() == 0) C.pop_back();
    
    return C;
}

int main(){
    string a;
    int b;
    cin >> a >> b;
    
    vector<int> A;
    for(int i=a.size()-1;i>=0; i--) A.push_back(a[i] - '0');
    
    auto C = mul(A,b);
    
    for(int i=C.size()-1; i>=0; i--) printf("%d", C[i]);
    
    return 0;
}
```

## [794. 高精度除法](https://www.acwing.com/problem/content/796/)

```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> div(vector<int> &A, int b, int &r){
        vector<int> C;
        r = 0; //余数
        for(int i = A.size() - 1; i>=0; i--){
            r = r * 10 + A[i];
            C.push_back(r / b);
            r %= b;
        }
        reverse(C.begin(), C.end());
        while(C.size() > 1 && C.back() == 0) C.pop_back();
        
        return C;
}

int main()
{
    string a;
    vector<int> A;

    int B;
    cin >> a >> B;
    for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');

    int r;
    auto C = div(A, B, r);

    for (int i = C.size() - 1; i >= 0; i -- ) cout << C[i];

    cout << endl << r << endl;

    return 0;
}
```

## [795. 前缀和](https://www.acwing.com/problem/content/797/)

```C++
#include<iostream>

using namespace std;

const int N = 100010;

int n,m;
int a[N],s[N]; //全局数组默认将所有元素初始化为0

int  main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++) scanf("%d", &a[i]);
    
    for(int i=1;i<=n;i++) s[i] = s[i-1] + a[i];// 前缀和的初始化
    
    while(m--){
        int l,r;
        cin >> l>> r;
        cout << s[r] - s[l-1] << endl;// 区间和的计算
    }
    
    return 0;
}
```

## [796. 子矩阵的和](https://www.acwing.com/problem/content/798/)

```C++
#include <iostream>

using namespace std;

const int N=1010;
int n,m,q;
int s[N][N];

int main(){
    cin >> n >> m >> q;
    
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            scanf("%d", &s[i][j]);
            
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            s[i][j] = s[i - 1][j] + s[i][j-1] - s[i-1][j-1];
        
    while(q--){
        int x1,y1,x2,y2;
        scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
        printf("%d\n",s[x2][y2]-s[x1-1][y2]-s[x2][y1-1]+s[x1-1][y1-1]);
    }
        
    return 0;
}
```





## [826. 单链表](https://www.acwing.com/problem/content/828/)

```C++
#include <iostream>

using namespace std;

const int N  = 100010;
//index 存储当前已经用到了哪个点 head表示头节点的下标 e[N]存储节点数据 ne[N]指向下一个元素
int index, head,e[N], ne[N];

void init(){
    index = 0;
    head = -1;
}
// 向头节插入一个数
void add_to_head(int x){
    e[index] = x;
    ne[index] = head;
    head = index;
    index++;
}
//删除第 k 次插入的数后面的数,即删除第k-1次插入后面的数（下标为ne[k-1]）
void delete_k(int k){
    ne[k-1] = ne[ne[k-1]];
}
//在第 k 个插入的数后插入一个数
void add_x(int k, int x){
    e[index] = x;
    ne[index] = ne[k-1];
    ne[k-1] = index;
    index++;
}

int main(){
    init();
    int m;
    cin >> m;

    while(m--){
        int k, x;
        char ch;
        cin >> ch;
        if(ch=='H'){
            cin >> x;
            add_to_head(x);
        }
        else if(ch=='D'){
            cin >> k;
            if(k==0) head = ne[head]; //k为0表示删除头节点
            else delete_k(k);
        }
        else if(ch=='I'){
            cin >> k >> x;
            add_x(k, x);
        }
    }
    for(int i=head;i!=-1;i=ne[i]){
        cout << e[i] << " ";
    }
 
    return 0;
}
```

## 