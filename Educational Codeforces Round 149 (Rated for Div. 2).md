### [Educational Codeforces Round 149 (Rated for Div. 2)](https://codeforces.com/contest/1837)



#### D. Bracket Coloring

用前缀和来代替栈进行括号匹配

逆转：**前缀和为负数**逆转后就能匹配

```c++
 for(int i = 1; i <= n; i ++ ){
        sum[i] = sum[i - 1] + (s[i] == '(' ? 1 : -1);
    }
```



