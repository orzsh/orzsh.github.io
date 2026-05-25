---
layout: post
title: LeetCode每日一题打卡
date: 2021-05-14 19:34:43
categories: [学习记录]
mathjax: true
tag: [LeetCode, 每日打卡]
comments: true
---

**LeetCode每日一题打卡开始于2021-05-14，预计打卡至大四毕业，总共两年多，每天都会打卡，但不一定会更新到这上面。我不想再当咸鱼了（逃。**

#### [12. 整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)

2021-05-14，数电课延后一周再上，上完我就要开始学了。

**方法一：模拟**

**思路**

根据罗马数字的唯一表示法，为了表示一个给定的整数*num*，我们寻找不超过*num*的最大符号值，将*num*减去该符号值，然后继续寻找不超过*num*的最大符号值，将该符号拼接在上一个找到的符号之后，循环直至*num*为0。最后的得到的字符即为*num*的罗马数字表示。

编程时，可以建立一个数值-符号对的列表*valueSymbols*，按数值从大到小排列。遍历*valueSymbols*中的每个数值-符号对，若当前数值*value*不超过*num*则从*num*中不断减去*value*，直至*num*小于*value*，然后遍历下一个数值-符号对。若遍历中*num*为0则跳出循环。

**代码**

```C++
const pair<int, string> valueSymbols[] = {
    {1000, "M"},
    {900, "CM"},
    {500, "D"},
    {400, "CD"},
    {100, "C"},
    {90, "XC"},
    {50, "L"},
    {40, "XL"},
    {10, "X"},
    {9, "IX"},
    {5, "V"},
    {4, "IV"},
    {1, "I"},
};
class Solution {
public:
    string intToRoman(int num) {
        string roman;
        for(const auto &[value, symbol] : valueSymbols){ //遍历数值-符号对
            while(num>=value){	//找到不超过当前数字的最大符号值
                num -= value;
                roman += symbol;
            }
            if(num == 0) break; //当数字为0时退出循环
        }
        return roman;
    }
};
```

**复杂度分析**

- 时间复杂度：*O*(1)。由于 *valueSymbols* 长度是固定的，且这 13 字符中的每个字符的出现次数均不会超过 3，因此循环次数有一个确定的上限。对于本题给出的数据范围，循环次数不会超过 15 次。

- 空间复杂度：*O*(1)。

#### [13. 罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

今天是2021-05-15，周六，可是为什么还有实验课啊，滑动变阻器坏了，实验没做出来，躺平~

**方法一：模拟**

**思路**

通常情况下，罗马数字中小的数字在大的数字的右边。若输入的字符串满足该情况，那么可以将每个字符视作一个单独的值，累加每个字符对应的数值即可。

例如`XXVII`可视作`X` + `X`+ `V`+ `I`+ `I` = 10 + 10 + 5 + 1 + 1 = 27。

若存在小的数字在大的数字的左边的情况，根据规则需要减去小的数字。对于这种情况，我们也可以将每个字符视作一个单独的值，若一个数字的右侧的数字比它大，则该数字的符号取反。

例如`XIV`可视作 `X` - `I` + `V`  = 10 - 1 + 5 = 14。

**代码**

```C++
class Solution {
public:
    int romanToInt(string s) {
    unordered_map<char, int> symbolValues = {
        {'I', 1},
        {'V', 5},
        {'X', 10},
        {'L', 50},
        {'C', 100},
        {'D', 500},
        {'M', 1000},
    };
    int ans = 0;
    int n = s.length();
    for(int i=0; i < n; i++){
        int value = symbolValues[s[i]];
        if(i<n-1 && value < symbolValues[s[i+1]]) //判断后面一个字符对应的值是否比当前这个的值大
            ans -= value;
        else
            ans += value;
    }
    return ans;        
    }
};
```

**复杂度分析**

- 时间复杂度：*O*(*n*)。其中n是字符串s的长度。

- 空间复杂度：*O*(*1*)。

#### [421. 数组中两个数的最大异或值](https://leetcode-cn.com/problems/maximum-xor-of-two-numbers-in-an-array/)

2021-05-16，周日，玩了一上午的OW（破坏球真好玩）。

**方法一：暴力**

题目要求在*O*(n)的时间复杂度下完成，但暴力解法时间复杂度会上升到*O*(n^2)，用C++就会超时，而Java却AC了(不是很懂.jpg)。

**代码**

```Java
class Solution {
        public static int findMaximumXOR(int[] nums) {
        int ret = 0;
        for (int i = 0; i < nums.length; i++) {
            for (int j = i; j < nums.length; j++) {
                ret = Math.max(nums[i]^nums[j], ret);
            }
        }
        return ret;
    }
}
```

**方法二：哈希表**

这个方法来自官方题解，自己想不出别的了。

**思路**

设最大异或值为m。[从高到低依次求m的每一个bit]

设x(k)表示x的从高到低的前k位。

如何求m？已经求出m(k-1)，判断第k是否为1：设m第k位可以为1的条件是：存在x和y于nums，满足m(k)=x(k)^y(k)，否则m第k位是0。

**代码**

```C++
class Solution {
public:
    int findMaximumXOR(vector<int>& nums) {
        int m = 0;
        for (int k=30; k>=0; --k){  // 从高到低依次求m的每一个bit。
            //保存num(k)
            unordered_set<int> set;   
            for (int num: nums){
                set.insert(num>>k);
            }
            // 检查是否存在x和y于nums，满足m(k)=x(k)^y(k)
            m = (m<<1)+1;   //假设当前位为1
            bool canBe1 = false;
            for (int num: nums){
                if (set.count((num>>k)^m)){
                    canBe1 = true;
                    break;
                }
            }
            if (!canBe1) --m;   // 不存在存在x和y于nums，说明m不能为1
        }
        return m;
    }
};
```

#### [993. 二叉树的堂兄弟节点](https://leetcode-cn.com/problems/cousins-in-binary-tree/)

2021-05-17，打卡第4天，上午有乒乓球课，玩得很过瘾啦~

**前言**

要想判断两个节点 `x`和 `y` 是否为堂兄弟节点，我们就需要求出这两个节点分别的「深度」以及「父节点」。

因此，我们可以从根节点开始，对树进行一次遍历，在遍历的过程中维护「深度」以及「父节点」这两个信息。当我们遍历到 x 或 y 节点时，就将信息记录下来；当这两个节点都遍历完成了以后，我们就可以退出遍历的过程，判断它们是否为堂兄弟节点了。

常见的遍历方法有两种：深度优先搜索和广度优先搜索。

**方法一：深度优先搜索**

**思路与算法**

我们只需要在深度优先搜索的递归函数中增加表示「深度」以及「父节点」的两个参数即可。

**代码**

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
private:
    //x的信息
    int x;
    TreeNode* x_parent;
    int x_depth;
    bool x_found = false;

    //y的信息
    int y;
    TreeNode* y_parent;
    int y_depth;
    bool y_found = false;

public:
    bool isCousins(TreeNode*& root, int& x, int& y) {
        this->x = x;
        this->y = y;
        dfs(root, 0 ,nullptr);
        return x_depth ==y_depth && x_parent != y_parent;
    }

    void dfs(TreeNode* node, int depth, TreeNode* parent){
        if(node==nullptr) return;
        if(node->val == x) tie(x_parent, x_depth, x_found) = tuple(parent, depth, true);
        else if(node->val == y) tie(y_parent, y_depth, y_found) = tuple(parent, depth, true); 
        
        //找到了两个节点就提前退出遍历，但不退出对最坏情况下的时间复杂度也不会有影响。
        if(x_found && y_found) return;

        dfs(node->left, depth+1, node);

        if(x_found && y_found) return;

        dfs(node->right, depth+1, node);
    }
};
```

**复杂度分析**

时间复杂度：O(n)，其中 n 是树中的节点个数。在最坏情况下，我们需要遍历整棵树，时间复杂度为 O(n)。

空间复杂度：O(n)，即为深度优先搜索的过程中需要使用的栈空间。在最坏情况下，树呈现链状结构，递归的深度为 O(n)。

**方法二：广度优先搜索**

**思路来自[Qian](https://leetcode-cn.com/u/qiank/)**

因为在BFS中，我们使用的是层序遍历，如果每次遍历一层，那么这一层的元素的深度是相同的。

因此我们在每一层，看看是否有出现 x 和 y，其中分为以下三种情况：

- `x` 和 `y`都没出现 → 那只能往下一层继续找了

- `x` 和 `y` 只出现一个 → 两个元素的深度不同，不可能为兄弟，返回`false`

- `x` 和 `y`都出现了，好耶，但是还不够好

  - `x` 和 `y` 父节点相同 → 不是堂兄弟，是亲兄弟，不行，返回`false`

  - `x` 和 `y`父节点不同 → 满足题目条件了，好耶，返回`true`

众所周知，BFS需要用到队列，那么我们应该如何设计队列的数据类型？
在这里，我采用了 `pair<TreeNode*, TreeNode*>`（其实`pair<TreeNode*, int>`也可以），其中`pair`中，第一个元素记录指向当前结点的指针，第二个元素记录指向当前结点的父结点的指针，这样就可以完美应对上面所说的三种情况了。

**代码**

```C++
class Solution{
public:
    using PTT = pair<TreeNode*, TreeNode*>;
    bool isCousins(TreeNode* root, int x, int y){
        queue<PTT> q; //q.first记录当前节点的指针，q.second记录当前节点的父节点的指针
        q.push({root, nullptr});
        while(!q.empty()){
            int n = q.size(); //记录当前层需要遍历的节点个数
            vector<TreeNode*> rec_parent; //记录每层找到的x,y的父节点
            for(int i = 0; i < n; i++){ //处理每层节点
                auto [cur, parent] = q.front(); //每循环一次就更新一次当前将要遍历的节点
                q.pop(); //取得这个节点后将其从队头弹出
                if(cur->val == x || cur->val == y) //检查当前节点的值是否与x,y之一相等
                    rec_parent.push_back(parent);
                if(cur->left) q.push({cur->left, cur}); //检查当前节点的左子节点是否存在，存在就加入队列，同时记录其父节点
                if(cur->right) q.push({cur->right, cur});//检查当前节点的右子节点是否存在，存在就加入队列，同时记录其父节点
            }

            //遍历完一层之后检查是否找到了x,y
            if(rec_parent.size() == 0) //x,y都没出现
                continue;
            else if(rec_parent.size() == 1) //x,y出现其一
                return false;
            else if(rec_parent.size() == 2) //x,y都出现，但还需判断父节点是否相同
                return rec_parent[0] != rec_parent[1]; //父节点不同则返回true
        }
        return false;
    }
};
```

**复杂度分析**

- 时间复杂度：O(n)，其中 n 是树中的节点个数。在最坏情况下，我们需要遍历整棵树，时间复杂度为 O(n)。

- 空间复杂度：O(n)，即为广度优先搜索的过程中需要使用的队列空间。

#### [1442. 形成两个异或相等数组的三元组数目](https://leetcode-cn.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/)

2021-05-18，轻松的一天，只有一节德语课。

**前言**

记S(n)  , n=0,1,2,3
$$
S(0) = 0, S(1) = arr[0], S(2) = arr[0] ⊕ arr[1], S(n) = arr[0] ⊕ arr[1] ⊕ ... ⊕ arr[n-1]
$$
为该数组的**异或前缀和**，由异或的性质可知：
$$
a = S(i-1) ⊕ S(j-1) = arr[i] ⊕ arr[i+1] ⊕ arr[i+2] ⊕ ... ⊕  arr[j-1]
$$

$$
b = S(j-1) ⊕ S(k) = arr[j-1] ⊕ arr[j] ⊕ arr[j+1] ⊕ ... ⊕  arr[k]
$$

**方法一：暴力**

**思路**

依次枚举`i`，`j`，`k`的值。


```C++
class Solution {
public:
    int countTriplets(vector<int>& arr) {
        int n = arr.size();
        vector<int> pre_arr(n+1); //保存异或前缀和
        for(int i = 1; i < n+1; i++){ //从1开始存贮异或前缀和方便理解
            pre_arr[i] = pre_arr[i-1] ^ arr[i-1];  
        }
        int a,b,ans=0;   
        for(int i=1;i<=n;i++)
            for(int j=i+1;j<=n;j++)
                for(int k=j;k<=n;k++){
                    a = pre_arr[i-1] ^ pre_arr[j-1];
                    b = pre_arr[j-1] ^ pre_arr[k];    
                    if (a==b) ans++; //ans += a==b;
                }
        return ans;
    } 
};
```

**复杂度分析**

时间复杂度：O(n^3)，其中 n 是数组 arr 的长度。

空间复杂度：O(n)

**方法二：二重循环**

**思路**

由异或的性质可知，
$$
a ⊕ b = 0时，arr[i] ⊕ ... ⊕ arr[j-1] ⊕ arr[j] ⊕ ... ⊕ arr[k] = 0
$$
因此在 i 之前的前缀异或和到 k 时不会变，即`pre_arr[i-1] = pre_arr[k]`。

其另一点重点在于在区间 `[i, k]`内 j在哪并不重要, 因为无论` j`在哪，`i`到`k`的异或值都等于 0，不影响结果。

```C++
class Solution {
public:
    int countTriplets(vector<int>& arr) {
        int n = arr.size(), ans = 0;
        vector<int> pre_arr(n+1);
        for(int i=1;i<n+1;i++)
            pre_arr[i] = pre_arr[i-1] ^ arr[i-1];
        for(int i=1;i<n;i++)
            for(int k=i+1;k<=n;k++){
                if((pre_arr[i-1] ^ pre_arr[k]) == 0) //pre_arr[i-1] == pre_arr[k]
                    ans += k-i;
            }
        return ans;
    }
};
```

**复杂度分析**

时间复杂度：O(n^2)

空间复杂度：O(n)

#### [1738. 找出第 K 大的异或坐标值](https://leetcode-cn.com/problems/find-kth-largest-xor-coordinate-value/)

2021-05-19，一看就会，一写就错是我了。

**思路**（来自官方题解）

我们用 ⊕ 表示按位异或运算。

由于「按位异或运算」与「加法运算」有着十分相似的性质，它们都满足交换律：
$$
a⊕b=b⊕a
$$
以及结合律：
$$
(a⊕b)⊕c=a⊕(b⊕c)
$$
因此我们可以使用「前缀和」这一技巧对按位异或运算的结果进行维护。由于本题中给定的矩阵 `matrix` 是二维的，因此我们需要使用二维前缀和。

设二维前缀和 `pre(i,j)` 表示矩阵 `matrix` 中所有满足 `0≤x<i` 且 `0≤y<j` 的元素执行按位异或运算的结果 。与一维前缀和类似，要想快速得到 `pre(i,j)`，我们需要已经知道 `pre(i−1,j)`，`pre(i,j−1)` 以及`pre(i−1,j−1)` 的结果，即：

$$
pre[i][j] = pre[i-1][j]⊕pre[i][j-1]⊕pre[i-1][j-1]⊕matrix[i-1][j-1];
$$
当我们将 `pre(i−1,j)` 和 `pre(i,j−1)` 进行按位异或运算后，由于对一个数 x 异或两次 `y`，结果仍然为 `x` 本身，即：
$$
x⊕y⊕y=x
$$
因此 `pre(i−1,j−1)` **对应区域的按位异或结果被抵消**，我们需要将其补上，并对位置 `(i,j)` 的元素进行按位异或运算，这样就得到了 `pre(i,j)`。

在得到了所有的二维前缀和之后，我们只需要找出其中第 k 大的元素即为答案。这一步我们可以直接将 `mn` 个二维前缀和进行排序后返第 `k` 大的元素。

**细节**

在二维前缀和的计算过程中，如果我们正在计算首行或者首列，即 `i=0` 或 `j=0`，此时例如 `pre(i−1,j−1)` 是一个超出下标范围的结果。因此我们可以使用一个 `(m+1)×(n+1)` 的二维矩阵，将**首行和首列空出来赋予默认值** `0`，并使用接下来的 `m` 行和 `n` 列存储二维前缀和，这样就不必进行下标范围的判断了。

**代码**

```C++
class Solution {
public:
    int kthLargestValue(vector<vector<int>>& matrix, int k) {
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> pre(m+1, vector<int>(n+1));
        vector<int> results;
        //求出二维异或前缀和，并用一个一维向量存贮结果
        for(int i=1;i<=m;i++)
            for(int j=1;j<=n;j++){
                pre[i][j] = pre[i-1][j]^pre[i][j-1]^pre[i-1][j-1]^matrix[i-1][j-1];
                results.push_back(pre[i][j]);
            }
        sort(results.begin(),results.end(),greater<int>()); //对异或前缀和从小到大排序
        return results[k-1];
    }
};
```

**复杂度分析**

- 时间复杂度：O(mnlog(mn))。计算二维前缀和的时间复杂度为 O(mn)，排序的时间复杂度为 O(mnlog(mn))，因此总时间复杂度为 O(mnlog(mn))。

- 空间复杂度：O(mn)，即为存储二维前缀和需要的空间。

#### [692. 前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/)

2021-5-20，有人相爱，有人夜里开车看海，有人520刷LeetCode (:

**方法一：哈希表 + 排序**
**思路及算法**

我们可以预处理出每一个单词出现的频率，然后依据每个单词出现的频率降序排序，最后返回前 `k` 个字符串即可。

具体地，我们利用哈希表记录每一个字符串出现的频率，然后将哈希表中所有字符串进行排序，排序时，如果两个字符串出现频率相同，那么我们让两字符串中字典序较小的排在前面，否则我们让出现频率较高的排在前面。最后我们只需要保留序列中的前 `k` 个字符串即可。

**代码**

```C++
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> map;
        vector<string> rec;

        for(auto& word:words)
            map[word]++;
        for(auto& [key, value] : map){
            rec.emplace_back(key);
        }

        sort(rec.begin(),rec.end(), [&](string& a, string& b){
            if(map[a]==map[b]) return a < b;
            else
                return map[a] > map[b];
        });
        /*
        sort(rec.begin(), rec.end(), [&](string& a, string& b) -> bool {
            return cnt[a] == cnt[b] ? a < b : cnt[a] > cnt[b];
        });     
        */

        rec.erase(rec.begin()+k,rec.end()); //擦除 第k个出现次数最多的单词 后面的所有单词,即剩下了前k个
        return rec;
    }
};
```

**复杂度分析**

- 时间复杂度：`O(l×n+l×mlogm)`，其中 `n` 表示给定字符串序列的长度，`l` 表示字符串的平均长度，`m` 表示实际字符串种类数。我们需要 `l×n` 的时间将字符串插入到哈希表中，以及 `l × mlogm` 的时间完成字符串比较（最坏情况下所有字符串出现频率都相同，我们需要将它们两两比较）。

- 空间复杂度：`O(l×m)`，其中 `l` 表示字符串的平均长度，`m` 表示实际字符串种类数。哈希表和生成的排序数组空间占用均为 `O(l×m)`。

#### [1035. 不相交的线](https://leetcode-cn.com/problems/uncrossed-lines/)

2021-5-21，对于今天来说，也许这题很奇怪（不相交

**思路：动态规划**

定义`dp[i][j]`表示数组`nums1`的前`i`个元素和`nums2`的前j个元素所能绘制的最大连接数，如果要求`dp[i][j]`，我们首先判断`nums1`的第`i`个元素和`nums2`的第`j`个元素是否相等

如果相等，说明`nums1`的第`i`个元素可以和`nums2`的第j个元素可以连成一条线，这个时候`nums1`的前`i`个元素和`nums2`的前j个元素所能绘制的最大连接数就是`nums1`的前`i-1`个元素和`nums2`的前`j-1`个元素所能绘制的最大连接数加`1`，也就是`dp[i][j]=dp[i-1][j-1]+1`;

如果不相等，我们就把`nums1`去掉一个元素，计算`nums1`的前`i-1`个元素和`nums2`的前j个元素能绘制的最大连接数，也就是`dp[i-1][j]`，或者把`nums2`去掉一个元素，计算`nums2`的前`j-1`个元素和`nums1`的前`i`个元素能绘制的最大连接数，也就是`dp[i][j-1]`，这两种情况我们取最大的即可，所以我们可以找出递推公式：

```C++
    if(nums[i] == nums[j])
        dp[i][j] = dp[i-1][j-1] + 1;
    else
        dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
```

**代码**

```C++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        //这里为了方便计算，减少一些边界条件的判断，把dp的宽高都增加1
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; ++i)
            for (int j = 1; j <= n; ++j)
                //下面是递推公式
                if (nums1[i - 1] == nums2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                else
                    dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
        return dp[m][n];
    }
};
```

**复杂度分析**

- 时间复杂度：O(mn)，其中 m 和 n 分别是数组 nums1和 nums2 的长度。二维数组 dp 有 m+1 行和  n+1 列，需要对 dp 中的每个元素进行计算。


- 空间复杂度：O(mn)，其中 m 和 n 分别是数组 nums1和 nums2 的长度。创建了 m+1 行 n+1 列的二维数组 dp。

#### [810. 黑板异或游戏](https://leetcode-cn.com/problems/chalkboard-xor-game/)

2021-5-22，一会儿是假新闻，一会儿又是证实，不是很懂央视的操作，袁老一路走好！

**思路**

要什么思路，我可不会！

**代码**

你问我代码哪来的？当然是CV啦~

```C++
class Solution {
public:
    bool xorGame(vector<int>& nums) {
        if (nums.size() % 2 == 0) {
            return true;
        }
        int xorsum = 0;
        for (int num : nums) {
            xorsum ^= num;
        }
        return xorsum == 0;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/chalkboard-xor-game/solution/hei-ban-yi-huo-you-xi-by-leetcode-soluti-eb0c/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

**复杂度分析**

- 时间复杂度：O(n)，其中 n 是数组 nums 的长度。最坏情况下，需要遍历数组一次，计算全部元素的异或结果。

- 空间复杂度：O(1)。

#### [1707. 与数组中元素的最大异或值](https://leetcode-cn.com/problems/maximum-xor-with-an-element-from-array/)

2021-5-23，一看困难，又是异或，那我走？

**代码**

CV啦~

```C++
class Trie {
public:
    const int L = 30;

    Trie* children[2] = {};
    int min = INT_MAX;

    void insert(int val) {
        Trie* node = this;
        node->min = std::min(node->min, val);
        for (int i = L - 1; i >= 0; --i) {
            int bit = (val >> i) & 1;
            if (node->children[bit] == nullptr) {
                node->children[bit] = new Trie();
            }
            node = node->children[bit];
            node->min = std::min(node->min, val);
        }
    }

    int getMaxXorWithLimit(int val, int limit) {
        Trie* node = this;
        if (node->min > limit) {
            return -1;
        }
        int ans = 0;
        for (int i = L - 1; i >= 0; --i) {
            int bit = (val >> i) & 1;
            if (node->children[bit ^ 1] != nullptr && node->children[bit ^ 1]->min <= limit) {
                ans |= 1 << i;
                bit ^= 1;
            }
            node = node->children[bit];
        }
        return ans;
    }
};

class Solution {
public:
    vector<int> maximizeXor(vector<int> &nums, vector<vector<int>> &queries) {
        Trie* t = new Trie();
        for (int val : nums) {
            t->insert(val);
        }
        int numQ = queries.size();
        vector<int> ans(numQ);
        for (int i = 0; i < numQ; ++i) {
            ans[i] = t->getMaxXorWithLimit(queries[i][0], queries[i][1]);
        }
        return ans;
    }
};
```

#### [1190. 反转每对括号间的子串](https://leetcode-cn.com/problems/reverse-substrings-between-each-pair-of-parentheses/)

2021-05-26，连着CV了好几天，我是废物，我是废物，我是废物。今天终于可以重拳出击了嘿嘿~

**方法一：栈**
**思路及算法**

对于括号序列相关的题目，通用的解法是使用递归或栈。本题中我们将使用栈解决。

我们从左到右遍历该字符串，使用字符串 str 记录当前层所遍历到的小写英文字母。对于当前遍历的字符：

如果是左括号，将 str 插入到栈中，并将 str 置为空，进入下一层；

如果是右括号，则说明遍历完了当前层，需要将 str 反转，返回给上一层。具体地，将栈顶字符串弹出，然后将反转后的 str 拼接到栈顶字符串末尾，将结果赋值给 str。

如果是小写英文字母，将其加到 str 末尾。

注意到我们仅在遇到右括号时才进行字符串处理，这样可以保证我们是按照从括号内到外的顺序处理字符串。

**代码**

```C++
class Solution {
public:
    string reverseParentheses(string s) {
        stack<string> stk;
        string str;
        for(auto& ch:s){
            if(ch=='('){
                stk.push(str);
                str = "";
            }
            else if(ch == ')'){
                reverse(str.begin(),str.end());
                str = stk.top() + str;
                stk.pop();       
            }
            else{
                str.push_back(ch);
            }
        }
        return str;
    }
};
```

**复杂度分析**

- 时间复杂度：O(n^2)，其中 n 为字符串的长度。栈的最大深度为 O(n)，每一层处理的时间复杂度主要为反转的时间复杂度，为 O(n)，因此总时间复杂度为 O(n^2)。

- 空间复杂度：O(n)，其中 n 为字符串的长度。对于任意时刻，字符串中的任意一个字符至多只被栈中的一个位置包含一次。

#### [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

2021-05-27，一觉醒来十点半，乍看是道简单题，我直接重拳出击。

**方法一：位计数**

C++（以及其他语言）中内置了计算二进制表达中 1 的数量的函数。在工程中，我们应该直接使用内置函数，但这里我们暂不使用。

**__builtin_popcount(x ^ y)；**

**代码**

```C++
class Solution {
public:
    int hammingDistance(int x, int y) {
        int t,res=0;
        t = x^y;
        while(t){
            if(t%2==1) res++;
            t/=2;
        }
        return res;
    }
};
```

**复杂度分析**

- 时间复杂度：O(logC)，其中 C是元素的数据范围，在本题中 logC=log2^31=31。

- 空间复杂度：O(1)。

**方法二：Brian Kernighan 算法**

**代码**

跳过0直接计算1的个数。

```C++
class Solution {
public:
    int hammingDistance(int x, int y) {
        int s = x ^ y, ret = 0;
        while (s) {
            s &= s - 1;
            ret++;
        }
        return ret;
    }
};
```

**复杂度分析**

- 时间复杂度：O(logC)，其中 CC 是元素的数据范围，在本题中logC=log2^31 =31。

- 空间复杂度：O(1)。

#### [477. 汉明距离总和](https://leetcode-cn.com/problems/total-hamming-distance/)

2021-05-28，打卡第15天，每天都要坚持呀！

首先想到的是暴力，提交就发现超时了，只能用其他方法了。

**方法一：按位统计**

**代码**

```C++
class Solution {
public:
    int totalHammingDistance(vector<int>& nums) {
        int total=0, size = nums.size();
        int one,zero;
        for(int i=0;i<30;i++){
            one = 0; 
            for(int &num : nums ){
                one += (num>>i) & 1;
            }
            zero = size-one;
            total += zero * one;
        }
        return total;
    }
};
```

**复杂度分析**：

- 时间复杂度：O(n⋅L)。其中 n 是数组 nums 的长度，L=30。
- 空间复杂度：O(1)。

#### [231. 2 的幂](https://leetcode-cn.com/problems/power-of-two/)

2021-5-30，美好的一天从早起开始。

**思路**

一个数 n 是 2 的幂，当且仅当 n 是正整数，并且 n 的二进制表示中仅包含 1 个 1。

**代码一**

```C++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > && ( n & (n - 1)) == 0;
    }
};
```

**代码二**

```C++

class Solution {
public:
    bool isPowerOfTwo(int n) {
        return n > 0 && (n & -n) == n;
    }
};
```

**复杂度分析：**

时间复杂度：O(1)

空间复杂度：O(1)

#### [342. 4的幂](https://leetcode-cn.com/problems/power-of-four/)

2021-05-31，乒乓球考试终于考完啦~

**方法一：二进制表示中 1 的位置**

**思路**

先判断是不是2的倍数，再判断是不是4的倍数。

**代码**

```C++
class Solution {
public:
    bool isPowerOfFour(int n) {
        int x = 0b101010101010101010101010101010101;
        return n>0 && (n&(n-1))==0 && (x&n)!=0;
    }
};
```

**复杂度分析**

时间复杂度：O(1)

空间复杂度：O(1）

**方法二：取模性质**

**思路**

4的幂有两条性质：

1. 二进制中只有一个1
2. 除以3余1

**代码**

```C++
class Solution {
public:
    bool isPowerOfFour(int n) {
        return n>0 && (n&(n-1))==0 && ((n%3)==1);
    }
};
```

**复杂度分析**

时间复杂度：O(1)

空间复杂度：O(1）

#### [523. 连续的子数组和](https://leetcode-cn.com/problems/continuous-subarray-sum/)

2021-06-02，昨天遇见一位说话很~~可爱~~（欠）的女生（聊天中发现，可爱这个词她觉得并不准确，可我觉得没问题呀，但我还是换成了她对自己的描述：欠 ○( ＾皿＾)っHiahiahia…），幸好要到了联系方式，否则我真的会挂念很久的。一开始也是怂了，其实只要敢迈出这一步真的不难...

**代码**

```C++
# 前缀
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        int m = nums.size();
        if (m < 2) {
            return false;
        }
        unordered_map<int, int> mp;
        mp[0] = -1;
        int remainder = 0;
        for (int i = 0; i < m; i++) {
            remainder = (remainder + nums[i]) % k;
            if (mp.count(remainder)) {
                int prevIndex = mp[remainder];
                if (i - prevIndex >= 2) {
                    return true;
                }
            } else {
                mp[remainder] = i;
            }
        }
        return false;
    }
};
```

**复杂度分析**

- 时间复杂度：O(m)，其中 m 是数组 nums 的长度。需要遍历数组一次。

- 空间复杂度：O(min(m,k))，其中 m 是数组 nums 的长度。空间复杂度主要取决于哈希表，哈希表中存储每个余数第一次出现的下标，最多有 min(m,k) 个余数。

#### [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

2021-06-04，走到尽头见不到你，于是走过你来时的路，等到相遇时才发现，你也走过我来时的路。

**思路**

利用集合，先遍历一遍链表A，将A中的所节点存入set，再遍历链表B，如果遇到当B中的某一节点出现在set中，说明该节点是相交节点，返回该节点即可，没找到返回`null`。

**代码**

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        unordered_set<ListNode *> Set;
        ListNode *p = headA;
        
        while(p){
            Set.insert(p);
            p=p->next;
        }
        
        p=headB;
        while(p){
            if(Set.find(p)!=Set.end())
                return p;
            p=p->next;
        }
        return nullptr;

    }
};
```

**复杂度分析**

- 时间复杂度：O(m+n)，其中 m 和 n 是分别是链表 headA 和 headB 的长度。两个指针同时遍历两个链表，每个指针遍历两个链表各一次。
- 空间复杂度：O(1)。

#### [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

**思路**

递归yyds

**代码**

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if(head==nullptr) return head;
        head->next = removeElements(head->next, val);
        if(head->val == val)
            return head->next;
        else
            return head;
        
    }
};
```

**复杂度分析**

- 时间复杂度：O(n)，其中 n 是链表的长度。递归过程中需要遍历链表一次。

- 空间复杂度：O(n)，其中 n 是链表的长度。空间复杂度主要取决于递归调用栈，最多不会超过 n 层。

#### [726. 原子的数量](https://leetcode-cn.com/problems/number-of-atoms/)

2021-07-05，一个月没更新了，今天终于想起来了，一看题，哦困难啊，告辞告辞。

**代码**

```C++
class Solution {
public:
    string countOfAtoms(string formula) {
        int i = 0, n = formula.length();

        auto parseAtom = [&]() -> string {
            string atom;
            atom += formula[i++]; // 扫描首字母
            while (i < n && islower(formula[i])) {
                atom += formula[i++]; // 扫描首字母后的小写字母
            }
            return atom;
        };

        auto parseNum = [&]() -> int {
            if (i == n || !isdigit(formula[i])) {
                return 1; // 不是数字，视作 1
            }
            int num = 0;
            while (i < n && isdigit(formula[i])) {
                num = num * 10 + int(formula[i++] - '0'); // 扫描数字
            }
            return num;
        };

        stack<unordered_map<string, int>> stk;
        stk.push({});
        while (i < n) {
            char ch = formula[i];
            if (ch == '(') {
                i++;
                stk.push({}); // 将一个空的哈希表压入栈中，准备统计括号内的原子数量
            } else if (ch == ')') {
                i++;
                int num = parseNum(); // 括号右侧数字
                auto atomNum = stk.top();
                stk.pop(); // 弹出括号内的原子数量
                for (auto &[atom, v] : atomNum) {
                    stk.top()[atom] += v * num; // 将括号内的原子数量乘上 num，加到上一层的原子数量中
                }
            } else {
                string atom = parseAtom();
                int num = parseNum();
                stk.top()[atom] += num; // 统计原子数量
            }
        }

        auto &atomNum = stk.top();
        vector<pair<string, int>> pairs;
        for (auto &[atom, v] : atomNum) {
            pairs.emplace_back(atom, v);
        }
        sort(pairs.begin(), pairs.end());

        string ans;
        for (auto &p : pairs) {
            ans += p.first;
            if (p.second > 1) {
                ans += to_string(p.second);
            }
        }
        return ans;
    }
};
```

