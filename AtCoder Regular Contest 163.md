# AtCoder Regular Contest 163

#### A - Divide String

贪心的发现分成2部分即可，枚举断点再比较，复杂度为$O(n^2)$

```cpp
void solve(){
    int n;
    cin >> n;

    string s;
    cin >> s;

    for(int i = 0; i < s.size() - 1; i ++ ){
        string a = s.substr(0, i + 1);
        string b = s.substr(i + 1);

        if(a < b) {
            cout << "Yes\n";
            return;
        }        
    }

    cout << "No\n";
}
```





#### B - Favorite Game

题目：

You are given an integer sequence of length N: A=(A1,A2,…,AN). You can perform the following operation any number of times (possibly zero).

- Choose an integer i such that 1≤i≤N, and increase or decrease Ai by 1.

Your goal is to make at least M integers i(3≤i≤N) satisfy A1≤Ai≤A2. Find the minimum number of operations required to achieve this goal.



思路：

将a1和a2挑出来，分别为l和r，其余元素排个序。

如果从l和r来入手，考虑l和r的移动，就会比较麻烦。**所以转化一下思路，迭代我们的答案区间**，就是从小到大来枚举选M个元素出来的情况，然后计算l和r操作的代价。最后找出最小的代价。

```cpp
void solve(){
    int n, m;
    cin >> n >> m;
    vector<int> a(n - 2);
    int l, r;
    cin >> l >> r;
    for(auto& v : a) cin >> v;

    sort(a.begin(), a.end());

    int ans = 1e18;
    for(int i = 0, j = m - 1; j < n - 2; i ++, j ++ ){
        int st = a[i], ed = a[j];
        int dis = max(0ll, l - st) + max(0ll, ed - r);
        ans = min(ans, dis);
    }

    cout << ans << "\n";
}
```





#### C - Harmonic Mean

**非常好题：构造 + 数学**

题意：

构造一个长度为n的序列$A=[A_1,A_2,...,A_n]$使得$\sum_{i=1}^{n}{\frac{1}{A_i}} = 1$



思路：

**核心为利用裂项相消公式           $\frac{1}{k*(k+1)}=\frac{1}{k} - \frac{1}{k+1}$**

首先可以构造出：

​			$\frac{1}{1*2} + \frac{1}{2*3} + \frac{1}{3*4} + ... + \frac{1}{(n-1)*n}+\frac{1}{n} = 1$

1. 如果不存在k，使得k*(k+1)=n，即最后的n不会导致重复，那么上面的序列就是答案。
2. 如果最后的n重复了，那么可以改为构造$\frac{1}{2} +\frac{1}{2}*1$，即：

​			$\frac{1}{2} + \frac{1}{2} *(\frac{1}{1*2} + \frac{1}{2*3} + \frac{1}{3*4} + ... + \frac{1}{(n-2)*(n-1)}+\frac{1}{n-1}) = 1$

​		其中括号中的和为1。

​		由于n-1与n互质，所以不会有元素与n-1重复。

```cpp
void solve(){
    int n;
    cin >> n;
    
    if(n == 1) {
        cout << "Yes\n1\n"; 
        return ;
    }
    if(n == 2){
        cout << "No\n";
        return ;
    }

    cout << "Yes\n";

    int f = 1;
    vector<int> ans;
    for(int i = 1; i < n; i ++ ){
        int x = i * (i + 1);
        if(x == n) f = 0;
        ans.push_back(x);
    }
    
    if(f == 0){     //这一段的实现也很妙，不需要按顺序
       ans.pop_back();
       ans.push_back(n - 1);
       for(auto &v : ans) v *= 2;
       ans.push_back(2);
    }
    else ans.push_back(n);


    for(auto v : ans)
        cout << v << " ";
    cout << "\n";
    return ;
}
```





#### D - Sum of SCC

##### [竞赛图](https://www.cnblogs.com/Go7338395/p/15399843.html)：

*n* 个点的竞赛图定义为将 *n*个点的无向完全图每条边任意定向所构成的有向图。

为什么称之为竞赛图呢？因为其存在一个组合意义：*n* 只队伍两两之间比赛，每场比赛从赢的向输的连边就构成了一张竞赛图。

**性质一：竞赛图缩点后呈链状**

> 竞赛图缩点会得到一条链，链上前面的点到后面的每个点都有边。



后面是一些计数问题，没怎么看懂
