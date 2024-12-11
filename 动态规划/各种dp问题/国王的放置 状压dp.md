# 国王的放置 状压dp

## 题目描述

在 n×n的棋盘上放个国王，国王可攻击相邻的格子，求使它们无法互相攻击的方案总数。

输入格式共一行，包含两个整数 n和k。

输出格式
共一行，表示方案总数，若不能够放置则输出0
。

数据范围
1≤n≤10,0≤k≤n2

#### 样例

```
输入样例：
3 2
输出样例：
16
```
---
本题发现每一行的国王只能影响下一行,所以我们以每一行的状态进行枚举,然后进行状态转移,最后统计答案即可.
设f[i][j][a]表示前i行，第i行状态为a，一共用了j个国王的方案数.
状态转移方程:
f[i][j][a]+=f[i-1][j-cnt[a]][b],b是上一行的任意的合法状态
同时也有一个优化过程,先预处理出一行之中的合法状态,然后同时记录每一种合法状态的国王数量.
然后预处理出每一种状态的上一行合法的状态,状态转移时直接调用即可.
最后直接统计答案即可.

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll N = 12, M = 1 << 10, K = 110;
ll n, m;
vector<ll> state; // 记录一行之间的合法状态
ll cnt[M];
vector<ll> head[M];
ll f[N][K][M];// f[i][j][a]表示前i行，第i行状态为a，共用了j个物品的方案数
bool check(ll state)//检查有没有相邻的国王
{
    for (ll i = 0; i < n; i++)
    {
        if ((state >> i & 1) && (state >>(i + 1) & 1))
            return false;
    }
    return true;
}
int count(int state)
{
    int res = 0;
    for (int i = 0; i < n; i++)
        res += ((state >> i) & 1);
    return res;
}
int main()
{
    cin >> n >> m;
    for (ll i = 0; i < (1 << n); i++)
    {
        if (check(i))
        {
            state.push_back(i);
            cnt[i] = count(i);//记录这个状态有多少个国王
        }
    }
    for (ll i = 0; i < state.size(); i++)
    {
        for (ll j = 0; j < state.size(); j++)
        {
            ll a = state[i], b = state[j];
            if ((a & b) == 0 && check(a | b))
                head[i].push_back(j);//记录第i行中上一行i-1行的合法状况
        }
    }
    //上面是预处理过程(要想想有什么数据是可以先预处理出来的,减少时间复杂度)
    f[0][0][0] = 1;
    for (ll i = 1; i <= n; i++)//每一排进行枚举
        for (ll j = 0; j <= m; j++)//国王数量进行枚举
            for (ll a = 0; a < state.size(); a++)//a的状态进行枚举
            {
                for (int b: head[a])//a的上一排合法状态进行枚举
                {
                    ll c = cnt[state[a]];
                    if (j >= c)
                        f[i][j][a] += f[i - 1][j - c][b];
                }
            }
    ll res = 0;
    for(int i=0;i<state.size();i++)//记录答案
        res+=f[n][m][i];
    cout << res << endl;
    return 0;
}
```




