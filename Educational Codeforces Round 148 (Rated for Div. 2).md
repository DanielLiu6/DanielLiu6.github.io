# [Educational Codeforces Round 148 (Rated for Div. 2)](https://codeforces.com/contest/1832)

#### B. Maximum Sum

​	题意：给一个序列，进行k次操作，每次操作有两种选择（1）找两个最小的元素并删去（2）找最大元素并删去。问最终序列最大为多少。

​	思维题，居然要想这么久

​	先排序。删去的元素是在排序后的序列的首位两侧，求出所有可能的分布，然后取最小的。

```cpp
void solve(){
    int n, k;
    cin >> n >> k;

    vector<int> a;
    int ans = 0;
    for(int i = 1; i <= n; i ++ ){
        int x;
        cin >> x;
        a.push_back(x);
        ans += x;
    }

    sort(a.begin(), a.end());

    vector<int> sum;
    int now = 0;

    for(int i = n - 1, j = k; j; i --, j -- ){
        now += a[i];
    }
    int minv = now;

    for(int i = 0, j = n - k; j < n; i += 2, j ++){
        now += a[i] + a[i + 1] - a[j];

        minv = min(minv, now);
    }


    cout << ans - minv << "\n";
}
```



**注意minv或者二分时r的初始值取1e9会小，很容易被坑，建议取1e18。l同理，注意是取0还是-1e18。**