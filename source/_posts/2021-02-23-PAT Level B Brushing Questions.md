---
layout: post
title: PAT (Basic Level) Practice （中文）刷题
date: 2021-2-23 15:41:04
categories: [学习记录]
tag: [PAT, 算法, 数据结构]

comments: true
---

因为自己刷题总是三天打鱼两天晒网，为了督促自己，我就把每次做完的题记录到这上面来，也许这样可以给我点刷题的动力（使用的语言有C/C++、Python）。

题目地址：https://pintia.cn/problem-sets/994805260223102976/problems/type/7



## **1001 害死人不偿命的(3n+1)猜想 (15 分)**

```python
# python
n = int(input())
count = 0
while(n != 1):
    if n%2==0:
        n /= 2
    else:
        n = (3*n + 1) / 2
    count = count + 1
    
print(count)
```

## **1002 写出这个数 (20 分)**

```python
# python
n = input()

sum = 0
for i in n:
    sum += eval(i)
pinyins = ("ling", "yi", "er", "san", "si","wu", "liu", "qi", "ba", "jiu")
for i,k in enumerate(str(sum)):
    if i==0:
        print(pinyins[int(k)], end='')
    else:
        print(' ' + pinyins[int(k)], end='')
```

## **1004 成绩排名 (20 分)**

```C++
// C++
// 这题是我胡乱写的，想到用类我就用了，也没想其他的，所以代码看起来很复杂。
#include <iostream>

using namespace std;

class Student{
    private:
        string name,num;
        int score;
    public:
        void set_name(){
            cin >> this->name;
        }
        void set_num(){
            cin >> this->num;
        }
        void set_score(){
            cin >> this->score;
        }
        string get_name(){return name;}
        string get_num(){return num;}
        int get_score(){return score;}
};

int main(){
    int n;
    cin >> n;
    class Student Students[n];
    
    int max=0, min=100;
    string tmp_name_max, tmp_name_min;
    string tmp_num_max, tmp_num_min;
    for(int i=0;i<n;i++){
        Students[i].set_name();
        Students[i].set_num();
        Students[i].set_score();
      
        if (Students[i].get_score() > max){
            max = Students[i].get_score();
            tmp_name_max = Students[i].get_name();
            tmp_num_max = Students[i].get_num();
        }
        if (Students[i].get_score() < min){
            min = Students[i].get_score();
            tmp_name_min = Students[i].get_name();
            tmp_num_min = Students[i].get_num();
        }
    }
    
    cout << tmp_name_max << " " << tmp_num_max << endl;
    cout << tmp_name_min << " " << tmp_num_min << endl;
    
    return 0;
}
```

## **1006 换个格式输出整数 (15 分)**

```C++
// C++

#include <iostream>

using namespace std;
int main(){
    int a,i=0;
    cin >> a;
    int n[3]={0};
    while(a){
        n[i++] = a%10;
        a = a/10;
    }
    for (int i=0; i < n[2]; i++){
        cout << 'B';
    }
    for (int i=0; i < n[1]; i++){
        cout << 'S';
    }
    for (int i=0; i < n[0]; i++){
        cout << i+1;
    }  
    return 0;
}
```

## **1007 素数对猜想 (20 分)**

```C++
// C++
#include <iostream>

using namespace std;

bool is_prime(int n);

int main(){
    int n, count=0;
    cin >> n;
    for (int i=5;i<=n;i++){
        if (is_prime(i-2)&&is_prime(i))
            count++;
    }
    cout << count;
    return 0;
}

bool is_prime(int n){
    for (int i=2;i*i<=n;i++){
        if (n%i==0) return false;
    }
    return true;
}
```

## **1008 数组元素循环右移问题 (20 分)**

```C++
#include <iostream>
using namespace std;
int main()
{
    int n[101]= {0};
    int m;
    int N;
    cin>>N>>m;
    if(m>N)
        m=m%N;
    for(int i=0; i+m<N; i++)
    {
        cin>>n[i+m];
    }
    for(int i=0; i<m; i++)
    {
        cin>>n[i];
    }
    for(int i=0; i<N; i++)
    {
        if(i!=N-1)
            cout<<n[i]<<" ";
        else
            cout<<n[i];
    }
    return 0;

}
```

## **1009 说反话 (20 分)**

```C++
// C++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string str[80]; //用一个数组记录每个单词
    int n=0;
    while(cin>>str[n++]){
        if (cin.get()=='\n') //输入回车时退出循环
            break;
    }
    for (int i=n-1;i>=0;i--){
        if(i!=0)
            cout << str[i] << " ";
        else
            cout << str[i];
    }
    return 0;
}
```

## **1010 一元多项式求导 (25 分)**

```C++
// C++
#include <iostream>

using namespace std;

int main(){
    int a,b;
    int flag=0; // 用来判断当前输出的数据是否为"0 0"且为第一组数据;flag==0时表示为第一组数
    while (cin >> a >> b){
        if(b!=0){
            if(flag==0)
                cout << a*b << " " << (b-1);
            else
                cout << " " << a*b << " " << (b-1);
            flag=1; // 当第一组输出不为"0 0"时，flag置为1
        }
        else if(flag==0&&b==0) // 如果第一组输出为"0 0"且后面没有数据则输出"0 0"
            cout << "0 0";
    }
    return 0;
}
```

## **1011 A+B 和 C (15 分)**

```C++
#include <iostream>
#include <string>
using namespace std;

string True_or_False(long int a, long int b,long int c);  // 注意使用long int，否则不能得满分

int main() {
    int n;
    cin >> n;
    long int a,b,c;
    for (int i=1;i<=n;i++){
        cin >> a >> b >> c;
        cout << "Case #" << i << ": " << True_or_False(a,b,c) << endl;
    }
    return 0;
}

string True_or_False(long int a, long int b,long int c){
    if (a+b>c)
        return "true";
    else
        return "false";
}
```

## **1012 数字分类 (20 分)**

写了一天也就弄了几题，基础实在太差了，写起来很吃力，今天就到这吧，打游戏去了（逃

```C++
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

void f1(vector<int> v);
void f2(vector<int> v);
void f3(vector<int> v);
void f4(vector<int> v);
void f5(vector<int> v);

int main() {
    int n;
    vector<int> v;
    while(cin >> n)
        v.push_back(n);
    
    f1(v);
    f2(v);
    f3(v);
    f4(v);
    f5(v);
    
    return 0;
}

void f1(vector<int> v){
    int sum = 0,count=0;
    for (int i=1;i<v.size();i++){
        if((v[i]%5==0)&&(v[i]%2==0)){
            sum += v[i];
            count++;
        }
    }
    if (count!=0)
        cout << sum << " ";
    else if(count==0)
        cout << "N ";
}

void f2(vector<int> v){
    int sum=0,count=0;
    for (int i=1;i<v.size();i++){
        if (v[i]%5==1){
            sum += v[i]*(int)pow(-1.0,(double)count);
            count++;
        }
    }
    if (count!=0)
        cout << sum << " ";
    else 
        cout << "N ";
}

void f3(vector<int> v){
    int count=0;
    for (int i=1;i<v.size();i++){
        if(v[i]%5==2) count++;
    }
    if (count!=0)
        cout << count << " ";
    else 
        cout << "N ";
}
void f4(vector<int> v){
    float average=0,count=0;
    for (int i=1;i<v.size();i++){
        if(v[i]%5==3){
            average += (float)v[i];
            count++;
        }
    }
    average /= count;
    if (count!=0)
        printf("%0.1f ",average);
    else 
        cout << "N ";
}
void f5(vector<int> v){
    int max=0,count=0;
    for (int i=1;i<v.size();i++)
        if (v[i]%5==4)
            if(v[i]>max){
                max = v[i];
                count++;
            }
    if (count!=0)
        cout << max;
    else 
        cout << "N";
}
```

下面是在GitHub上找到的更棒的方法，居然直接在输入的时候就分类了，不得不说真是妙啊！

```C++
#include <iostream>
#include <vector>
using namespace std;
int main() {
    int n, num, A1 = 0, A2 = 0, A5 = 0;
    double A4 = 0.0;
    cin >> n;
    vector<int> v[5];
    for (int i = 0; i < n; i++) {
        cin >> num;
        v[num%5].push_back(num);
    }
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < v[i].size(); j++) {
            if (i == 0 && v[i][j] % 2 == 0) A1 += v[i][j];
            if (i == 1 && j % 2 == 0) A2 += v[i][j];
            if (i == 1 && j % 2 == 1) A2 -= v[i][j];
            if (i == 3) A4 += v[i][j];
            if (i == 4 && v[i][j] > A5) A5 = v[i][j];
        }
    }
    for (int i = 0; i < 5; i++) {
        if (i != 0) printf(" ");
        if (i == 0 && A1 == 0 || i != 0 && v[i].size() == 0) {
            printf("N"); continue;
        }
        if (i == 0) printf("%d", A1);
        if (i == 1) printf("%d", A2);
        if (i == 2) printf("%d", v[2].size());
        if (i == 3) printf("%.1f", A4 / v[3].size());
        if (i == 4) printf("%d", A5);
    }
    return 0;
}
```

## **1013 数素数 (20 分)**

```C++
#include <iostream>
using namespace std;

bool is_prime(int n);
int main(){
    int m,n,num=0,i=2;

    cin >> m >> n;
    while(num<=n){
        if(is_prime(i)){
            num++; // 每找到一个素数num就++
            if(num>=m&&num<n){
                if((num-m+1)%10==0)
                    cout << i << "\n"; //每十个素数输出一个换行
                else
                    cout << i << " ";
            }
            else if(num==n){
                if((num-m+1)%10==0)
                    cout << i << "\n";
                else
                    cout << i;
            }
        }
        i++;
    }
    return 0;
}

bool is_prime(int n){
    for (int i=2;i*i<=n;i++){
        if(n%i==0) return false;
    }
    return true;
}
```

## **1014 福尔摩斯的约会 (20 分)**

```C++
#include <iostream>
#include <cctype>
#include <string>
using namespace std;

int main(){
    string a,b,c,d;
    cin >> a >> b >> c >> d;
    
    // 先处理前两句
    char ch[2];
    int i=0;
    for(i; i<a.length() && i<b.length(); i++){
        if(a[i] == b[i] && (a[i] >= 'A' && a[i] <= 'G')){
            ch[0] = a[i];
            break;
        }
    }
    i = i+1;
    for(i; i<a.length() && i<b.length(); i++){
        if (a[i]==b[i] && ((a[i] >= 'A' && a[i] <= 'N') ||isdigit(a[i]))){
            ch[1] = a[i];
            break;
        }
     
    }
    
    // 再处理后两句
    int j = 0, pos;
    for (j;j<c.length() && j<d.length();j++){
        if(c[j] == d[j] && isalpha(c[j])){
            pos = j;
            break;
        }
    }
    
    // 输出
    string week[7] = {"MON ", "TUE ", "WED ", "THU ", "FRI ", "SAT ", "SUN "};
    cout << week[ch[0]-'A'];
    int m = isdigit(ch[1]) ? ch[1] - '0' : ch[1] - 'A' + 10;
    printf("%02d:%02d", m, pos);
    return 0;
}
```

## **1015 德才论 (25 分)**

```C++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
struct node {
    int num, de, cai;
};
int cmp(struct node a ,struct node b){
    if ((a.de + a.cai) != (b.de + b.cai))
        return (a.de + a.cai) > (b.de + b.cai);
    else if (a.de != b.de)
        return a.de > b.de;
    else
        return a.num < b.num;
}

int main(){
    int n, low, high;
    cin >> n >> low >> high;
    vector<node> v[4];
    node temp;
    int total = n;
    for(int i=0; i<n; i++){
        cin >> temp.num >> temp.de >> temp.cai;
        if(temp.de < low || temp.cai < low)
            total --;
        else if(temp.de >= high && temp.cai >= high)
            v[0].push_back(temp);
        else if(temp.de >= high && temp.cai < high)
            v[1].push_back(temp);
        else if(temp.de < high && temp.cai < high && temp.de >= temp.cai)
            v[2].push_back(temp);
        else
            v[3].push_back(temp);
    }
    cout << total << endl;
    for (int i=0; i<4;i++){
        sort(v[i].begin(), v[i].end(), cmp);
        for (int j=0; j<v[i].size(); j++)
            cout << v[i][j].num << " " << v[i][j].de << " " << v[i][j].cai << endl;
    }
    return 0;
}
```

## **1016 部分A+B (15 分)**

```C++
#include <iostream>
using namespace std;
int new_int_P(int a, int D);
int main(){
    int A,Da,B,Db;
    cin >> A >> Da >> B >> Db;
    int PA=0, PB=0;
    PA = new_int_P(A, Da);
    PB = new_int_P(B, Db);
    cout << PA + PB;
    return 0;
}
int new_int_P(int Da, int A){
    int P=0;
    while(Da){
        if((Da%10)==A)
            P = P*10+A;
        Da /= 10;
    }
    return P;
}
```

## **1017 A除以B (20 分)**

```C++
#include <iostream>
using namespace std;
int main() {
    string s;
    int a, t = 0, temp = 0;
    cin >> s >> a;
    int len = s.length();
    t = (s[0] - '0') / a;
    if ((t != 0 && len > 1) || len == 1)
        cout << t;
    temp = (s[0] - '0') % a;
    for (int i = 1; i < len; i++) {
        t = (temp * 10 + s[i] - '0') / a;
        cout << t;
        temp = (temp * 10 + s[i] - '0') % a;
    }
    cout << " " << temp;
    return 0;
}
```

## **1018 锤子剪刀布 (20 分)**

这题感觉写的真是稀烂啊。

```C++
#include <iostream>
using namespace std;
int main(){
    int p; //总次数
    char x,y; //甲乙出拳
    int a[3]={0}, b[3]={0}; //记录胜、平、负次数
    int m[3]={0}, n[3]={0}; //记录甲乙胜利时的手势
    cin >> p;
    while(p--){
        cin >> x >> y;
        int z = x - y;
        switch (z){
            case -7 : //甲赢
                a[0]++;
                b[2]++;
                if(x=='B')
                     m[0]++;
                else if(x=='C')
                     m[1]++;
                else if(x=='J')
                     m[2]++;
                break;
            case 7 : //乙赢
                a[2]++;
                b[0]++;
                if(y=='B')
                     n[0]++;
                else if(y=='C')
                     n[1]++;
                else if(y=='J')
                     n[2]++;
                break;
            case -1 : //甲赢
                a[0]++;
                b[2]++;
                if(x=='B')
                     m[0]++;
                else if(x=='C')
                     m[1]++;
                else if(x=='J')
                     m[2]++;
                break;
            case 1 : //乙赢
                a[2]++;
                b[0]++;
                if(y=='B')
                     n[0]++;
                else if(y=='C')
                     n[1]++;
                else if(y=='J')
                     n[2]++;
                break;
            case 8 : //甲赢
                a[0]++;
                b[2]++;
                if(x=='B')
                     m[0]++;
                else if(x=='C')
                     m[1]++;
                else if(x=='J')
                     m[2]++;
                break;
            case -8 : //乙赢
                a[2]++;
                b[0]++;
                if(y=='B')
                     n[0]++;
                else if(y=='C')
                     n[1]++;
                else if(y=='J')
                     n[2]++;
                break;
            case 0: //平
                a[1]++;
                b[1]++;
                break;
        }
    }
    cout << a[0] << " " << a[1] << " " << a[2] << endl;
    cout << b[0] << " " << b[1] << " " << b[2] << endl;
    
    int max_jia = m[0] >= m[1] ? 0 : 1;
    max_jia = m[max_jia] >= m[2] ? max_jia : 2;
    int max_yi = n[0] >= n[1] ? 0 : 1;
    max_yi = n[max_yi] >= n[2] ? max_yi : 2;
    char str[] = {"BCJ"};
    cout << str[max_jia] << " " << str[max_yi];
    
}
```

## **1019 数字黑洞 (20 分)**
```C++
#include<bits/stdc++.h>
using namespace std;
int cmp(char a, char b){return a>b;}
int main() {
    string s;
    cin >> s;
    s.insert(0, 4-s.length(), '0');
    do{
        string a=s,b=s;
        sort(a.begin(),a.end(),cmp);
        sort(b.begin(),b.end());
        int result = stoi(a) - stoi(b);
        s = to_string(result);
        s.insert(0,4-s.length(),'0');
        cout << a << " - " << b << " = "<< s <<endl;
    }while(s!="6174"&&s!="0000");
    cin.get();
    cin.get();
    return 0;
}
```

## **1020 月饼 (25 分)**

```C++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
struct mooncake{
    float mount, price, unit;
};
int cmp(mooncake a, mooncake b) {return a.unit>b.unit;}
int main(){
    int N,D;
    cin >> N >> D;
    vector<mooncake> a(N);
    for(int i=0;i<N;i++){
        cin >> a[i].mount;
    }
    for(int i=0;i<N;i++){
        cin >> a[i].price;
        a[i].unit=a[i].price/a[i].mount;
    }
    sort(a.begin(),a.end(),cmp);
    float result=0.0;
    for (int i=0;i<N;i++){
        if(a[i].mount<=D){
            result += a[i].price;
        }
        else{
            result += a[i].unit*D;
            break;
        }
        D = D-a[i].mount;
    }
    printf("%.2f",result);
    return 0;
}
```

## **1021 个位数统计 (15 分)**
```C++
#include<iostream>
#include<string>
using namespace std;
int main(){
    string N;
    cin >> N;
    int a[10]={0};
    for(int i = 0;i<N.length();i++){
        a[N[i]-'0']++;
    }
    for(int i=0;i<10;i++)
        if(a[i]!=0)
            printf("%d:%d\n",i,a[i]);
    
    return 0;
}
```

## **1022 D进制的A+B (20 分)**

```C++
#include<iostream>
#include<string>
using namespace std;
int main(){
    int a,b,d;
    cin >> a >> b >> d;
    int sum = a+b;
    if (sum==0) {cout << 0; return 0;}
    int s[100],i=0;
    while(sum){
        s[i++]=sum%d;
        sum/=d;
    }
    for (int j=i-1;j>=0;j--)
        cout << s[j];
    return 0;
}
```

## **1023 组个最小数 (20 分)**

```C++
#include <iostream>
using namespace std;

int main(){
    int arr[10]={0};
    for(int i=0;i<10;i++){
        cin >> arr[i];
    }
    int flag = 0; //标记是否输出过0
    for(int i=1;i<=9;i++){
        if(arr[i]>=1){
            cout << i;
            arr[i]--;
            
            if(flag == 0){ //检查0是否输出过
                for(int j=0;j<arr[0];j++){ //输出0
                    cout << 0;
                }
                flag = 1; //设为1表示已经输出过0
            }
            
            for(int j=0;j<arr[i];j++){ //输出对应位置的数
                cout << i;
            }
            
        }
    }
    return 0;
}
```

## **1024 科学计数法 (20 分)**

```C++
#include <iostream>
using namespace std;
int main() {
    string s;
    cin >> s;
    int i = 0;
    while (s[i] != 'E') i++;
    string t = s.substr(1, i-1);
    int n = stoi(s.substr(i+1));
    if (s[0] == '-') cout << "-";
    if (n < 0) {
        cout << "0.";
        for (int j = 0; j < abs(n) - 1; j++) cout << '0';
        for (int j = 0; j < t.length(); j++)
            if (t[j] != '.') cout << t[j];
    } else {
        cout << t[0];
        int cnt, j;
        for (j = 2, cnt = 0; j < t.length() && cnt < n; j++, cnt++) cout << t[j];
        if (j == t.length()) {
            for (int k = 0; k < n - cnt; k++) cout << '0';
        } else {
            cout << '.';
            for (int k = j; k < t.length(); k++) cout << t[k];
        }
    }
    return 0;
}
```

## 1026 程序运行时间 (15 分)

```C++
#include <iostream>

using namespace std;
int main(){
    int c1,c2;
    cin >> c1 >> c2;
    int time = (c2-c1+50)/100; //50 is added for rounding
    
    int hh = time / 3600;
    time %= 3600;
    int mm = time / 60;
    time %= 60;
    int ss = time;
    
    printf("%02d:%02d:%02d",hh, mm, ss);
    return 0;
}
```

## 1028 人口普查 (20 分)

```C++
#include <iostream>
using namespace std;

int main(){
    int n, count = 0;
    cin >> n;
    string name, birth, max_name, min_name, max_birth="1814/09/06", min_birth="2014/09/06";
    for(int i=0;i<n;i++){
        cin >> name >> birth;
/*
两个字符串自左向右逐个字符相比（按ASCII值大小相比较），直到出现不同的字符或遇’\0’为止。
当两个数的位数一样，则直接可以应用字符串的比较。
*/
        if(birth>="1814/09/06"&&birth<="2014/09/06"){
            count++;
            if(birth >= max_birth){
                max_name = name;
                max_birth = birth;
            }
            if(birth <= min_birth){
                min_name = name;
                min_birth = birth;
            }
        }
    }
    cout << count;
    if(count!=0) cout << " " << min_name << " " << max_name;
    return 0;
}
```

## 1036 跟奥巴马一起编程 (15 分)

```C++
#include <iostream>
using namespace std;

int main(){
    int n;
    string ch;
    cin >> n >> ch;
    int t=n/2 + n%2; //四舍五入
    for(int i=0;i<n;i++)
        cout << ch;
    cout << endl;
    for(int i=0;i<t-2;i++){ //上下的一行ch单独输出
        cout << ch;
        for(int i=0;i<n-2;i++) //左右的ch单独输出
            cout << " ";
        cout << ch << endl;
    }
    for(int i=0;i<n;i++)
        cout << ch;
    cout << endl;
    return 0;
}
```

## 1086 就不告诉你 (15 分)

```C++
#include <iostream>
#include <vector>
using namespace std;

int main(){
    int a,b;
    vector<int> v;
    cin >> a >> b;
    int mult = a*b;
    while (mult){
        v.push_back(mult%10);
        mult /= 10;
    }
    int flag=0;
    for (int i=0;i<v.size();i++){
        if(v[i]!=0) flag=1;
        if(flag==1)
            cout << v[i];
    }
    return 0;
}
```

## 1087 有多少不同的值 (20 分)

```C++
#include<iostream>
#include <set>
int sum(int n){
    int sum = 0;
    sum = n/2 + n/3 + n/5;
    return sum;
}
using namespace std;
int main(){
    int N;
    cin >> N;
    set<int> s;
    for(int i=2;i<=N;i++){
        s.insert(sum(i));
    }
    cout << s.size()+1;
    return 0;
}
```

## 1091 N-自守数 (15 分)

```C++
#include <iostream>
#include <string>

using namespace std;
int main(){
    int m;
    cin >> m;
    while(m--){
        int k, flag = 0;
        cin >> k;
        for(int n=1;n<10;n++){
            int mul = n*k*k;
            string s_mul = to_string(mul), s_k = to_string(k);
            string s_mul_end = s_mul.substr(s_mul.length() - s_k.length());
            if(s_mul_end == s_k){
                cout << n << " " << mul << endl;
                flag = 1;
                break;
            }
        }
        if(flag == 0) cout << "No" << endl;
    }
    return 0;
}
```

## 1092 最好吃的月饼 (20 分)

```C++
#include<iostream>
#include <vector>
using namespace std;
int a[1500][105], sum[1005];
int main(){
    int m,n,max_n=0;
    cin >> m >> n;
    vector<int> ans;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            cin >> a[i][j];
            sum[j] += a[i][j];
            max_n = max(max_n,sum[j]);
        }
    }
    cout << max_n << endl;
    for(int i=1; i<=m;i++)
        if(sum[i]==max_n) ans.push_back(i);
    for(int i=0;i<ans.size();i++){
        if(i!=0) cout << " ";
        cout << ans[i];
    }
    return 0;
}

/*思路：
1、输入
2、算出每种月饼在各个城市售出的总数并记录在一个新的数组sum[1005]中
3、在sum中找出最大的，然后输出
4、输出最大销量对应的所有种类
*/
```

## 1093 字符串A+B (20 分)

```C++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string a,b,s;
    int hash[150]={0};
    getline(cin, a);
    getline(cin, b);
    s = a+b;
    for(int i=0;i<s.size();i++){
        if(hash[s[i]]==0) cout << s[i];
        hash[s[i]]++; //遇到相同的字符就++，可以统计出有几个相同的
    }
    return 0;
}
```

## 1094 谷歌的招聘 (20 分)

```C++
#include <iostream>
#include <string>
using namespace std;
bool is_prime(int n){
    if(n==1||n==0) return false;
    for(int i=2;i*i<n;i++){
        if(n%i==0) return false;
    }
    return true;
}

int main(){
    int L, K;
    cin >> L >> K;
    string S;
    cin >> S;
    for(int i=0;i<=L-K;i++){
        string t = S.substr(i, K);
        int num = stoi(t);
        if(is_prime(num)){
            cout << t;
            return 0;
        }
    }
    cout << "404\n";
    return 0;
}
```

未完待续...