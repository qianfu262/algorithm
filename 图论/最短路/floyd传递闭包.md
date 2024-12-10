# floyd传递闭包

## 1. 算法思想
传递闭包就是看通过若干次的中转，能否从某个点到达另一个点。
时间复杂度为O( $n^3$ )。

其中dp[i][j]就是i能否到达j。
```cpp
for(int k = 1; k <= n; ++k)
    for(int i = 1; i <= n; ++i)
        for(int j = 1; j <= n; ++j)
            dp[i][j] |= dp[i][k] & dp[k][j];
```

## 对算法进行优化
优化就是要运用位运算操作符，将二维数组优化为一维数组。
dp[i][k]==1表示i可以到达k,反之则是0,不能到达。
先判断i是否可以到达k,若可以,则将k能到达的点都赋值为1。


```cpp
bitset<N> dp[N];
void Floyd()
{
    for(int k = 1; k <= n; ++k)
        for(int i = 1; i <= n; ++i)
            if(d[i][k])
                d[i] |= d[k]; 
}

```