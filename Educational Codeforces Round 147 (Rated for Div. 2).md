# [Educational Codeforces Round 147 (Rated for Div. 2)            ](https://codeforces.com/contest/1821)

#### C. Tear It Apart

枚举剩下的字母为 a~z 的情况









#### D. Black Cells

区域赛风格的一道题

题意：

给定你n个区间，只有在这n个区间中你才可以进行染色。你需要染不低于k个，求所需的最小操作次数。 其中，每次操作你可以选择

- 将自己由x移动到x + 1
- 按下Shift
- 松开Shift

你按下Sift时就可以对该位置处进行染色，松开染色结束。



题解：

经过推导可以发现长度大于1的区间不跳过是最佳的

> Let's prove that it's not optimal to skip segments with length *l**e**n*=*r*[*i*]−*l*[*i*]+1≥2. By contradiction, suppose the optimal answer *a* has a skipped segment [*$l_i$*,*$r_i$*]. If we color that segment instead, we will make 2 more moves for pressing and releasing Shift, but we can make at least *l**e**n* right move less. So the new answer *a*′=*a*+2−*l**e**n*≤*a* — the contradiction.



接着分三类讨论

![image-20230719132456760](C:\Users\86153\AppData\Roaming\Typora\typora-user-images\image-20230719132456760.png)



```cpp
void solve(){
    cin >> n >> k;

    int len = 0;
    for(int i = 1; i <= n; i ++)
        cin >> l[i];
    for(int i = 1; i <= n; i ++ )
        cin >> r[i], len += r[i] - l[i] + 1;

    if(len < k){
        puts("-1");
        return ;
    }

    int ans = 1e18;
    int c = 0, s = 0;       //c为长度1的段数，s为长度大于1的段的长度和
    for(int i = 1; i <= n; i ++ ){
        if(r[i] - l[i] + 1 == 1) c ++;
        else s += r[i] - l[i] + 1;

        if(s + c < k) continue;
        else if(s < k && s + c >= k){
            ans = min(ans, r[i] + 2 * (i - c + k - s));
        }
        else if(s >= k){
            ans = min(ans, r[i] - (s - k) + 2 * (i - c));
            break;
        }
    }

    cout << ans << "\n";
}
```





#### E. Rearrange Brackets

好题，括号问题，也是区域赛风格。 

括号不一定要往dp方面想，要注意给定的括号序列是否是**regular**。



对于括号序列((())())，我们观察发现对于一对相邻的括号()，它对于题目的贡献就是它外面有几层括号包着它。对于加粗括号((())**()**) 的贡献是1， 因为它外面只有一层括号。对于((**()**)()) 它的贡献是2 ，因为它外层有两层括号。对于(**(**()**)**())的贡献是1，因为它外面只有一层括号。

> 补充：虽然题目中的贡献是指消除括号右边的所有括号数，但通过消除的顺序，可以把 括号数 转换为 外层括号数



我们考虑建树， 对于((())())给每个左括号一个标号 1 2 3 ））4 ）），我们建树，儿子与父亲表示包含关系。可以用stack实现这个过程。

![image-20230719153317859](C:\Users\86153\AppData\Roaming\Typora\typora-user-images\image-20230719153317859.png)

不难发现贡献就是深度。

接下来我们考虑操作。每次可以移动一个括号的位置。也就是说我们每次可以解除一层括号。那我们肯定是解除拥有[节点数](https://www.zhihu.com/search?q=节点数&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A"623673936"})

最多的树（贪心）。对答案的贡献就是 ans -= sz[v] - 1;我们可以用优先队列来每次找出sz[v]最大的树。

![image-20230719153558052](C:\Users\86153\AppData\Roaming\Typora\typora-user-images\image-20230719153558052.png)

```cpp
int n, k;
int root[N], son[N];
vector<int> e[N];
int ans = 0;

int dfs(int x){
    son[x] = 1;
    for(auto v : e[x])
        son[x] += dfs(v);
    ans += son[x] - 1;
    return son[x];
}

void solve(){
    int k;
    cin >> k;
    string s;
    cin >> s;

    for(int i = 0; i < s.size(); i ++ ){
        son[i] = 0, root[i] = 0, e[i].resize(0);
    }

    int idx = 0;
    stack<int> stk;
    for(int i = 0; i <= s.size(); i ++ ){
        if(s[i] == '('){
            idx ++;
            if(stk.size()){
                int fa = stk.top();
                e[fa].push_back(idx);
            }
            else root[idx] = 1;
            stk.push(idx);
        }
        else if(s[i] == ')') stk.pop();
    }


    priority_queue<pii> q;
    ans = 0;
    for(int i = 1; i <= idx; i ++ ){
        if(root[i]){
            dfs(i);
            q.push({son[i], i});
        }
    }


    while(k -- && q.size()){
        auto h = q.top();

        q.pop();

        ans -= h.x - 1;
  
        for(auto v : e[h.y]){
            q.push({son[v], v});
        }
    }

    cout << ans << "\n";
}

```

