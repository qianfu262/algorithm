# spfa求解差分约束问题

## 差分约束问题

对所有形如 $x_i-x_j\leq c_k$ 的约束条件，求一组解，使得所有约束条件都成立。
对于每一个这个约束条件,我们都可以转化为最短路,或者最长路问题求解.
转化: $x_i-x_j\leq c_k$ 视为从j号点向i号点连接了一条边权位 $c_k$ 的边.根据最短路的定义,一定有dist[i]<=dist[j]+ $c_k$ ,所以一定有 $x_i-x_j\leq c_k$ .

## 实例 洛谷P3275 糖果
思路:
(1)求方程组可行解
原点需要满足的要求:从原点出发,一定要可以到达所有点
对于一个不等式,xi<=xj+c;
步骤:
1.将不等式转化为一条xj->xi边,边权为c
2.找一个虚拟原点,使得该原点可以遍历到所有的边(**很多题目都可以建立一个虚拟原点,这样不用遍历每一个点**)
3.从原点求一遍最短路
结果一:存在负环则无解
结果二:如果没有负环,那么dist[i]就是不等式组的可行解
等价于有一条j->i的边,边权为c, $dist[j] \geq dist[i]+c$ ;
(2)求一个最大的可行解或者最小的可行解
结论:**最小值求最长路;最大值求最短路**
如何转化xi<=c这种条件:方法:建立一个原点,0,然后建立0->i的边,边权为c;
以求 $x_i$ 最大值为例:所有从xi出发,构成的不等式链, $x_i<=x_j+c_1 \leq x_k+c_2+c_1 \leq... \leq x_0+c_1+c_2+...+c_n ;(x_0=0)$ ,所计算出的上界,取所有上界的最小值(即求最短路),就是所求的最大的可行解. 



```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10, M = 3e5 + 10;
typedef long long ll;
int n, m;
int h[N], e[M], ne[M], w[M], idx;
ll dist[N];
int cnt[N]; // cnt判断负环
stack<int> q;
bool st[N];
void add(int a, int b, int c)
{
    e[idx] = b,w[idx]=c,ne[idx] = h[a], h[a] = idx++;
}
bool spfa()
{
    memset(dist, -0x3f, sizeof dist); // 求最长路
    dist[0] = 0;
    q.push(0);//虚拟原点
    st[0] = true;
    while (!q.empty())
    {
        int t = q.top();
        q.pop();
        st[t] = false;
        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] < dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n + 1) // 加上新创造的原点
                    return false;
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    return true;
}
int main()
{
    cin >> n >> m;
    memset(h, -1, sizeof h);
    memset(st, false, sizeof st);
    while (m--)
    {
        int x, a, b;
        cin >> x >> a >> b;
        if (x == 1)
            add(b, a, 0), add(a, b, 0);//要求一样多分解为,a<=b, b<=a
        else if (x == 2)
            add(a, b, 1);//要求a<b,分解为a<=b+1
        else if (x == 3)//要求a>b,分解为b<=a+1
            add(b, a, 0);
        else if (x == 4)//要求a>=b,分解为a<=b
            add(b, a, 1);
        else
            add(a, b, 0);//要求a<=b,分解为a<=b
    }
    for (int i = 1; i <= n; i++)
        add(0, i, 1);//建立虚拟原点和每个点的一条边
    if (!spfa())
    {
        cout << -1 << endl;
    }
    else
    {
        ll res = 0;
        for (int i = 1; i <= n; i++)
            res += dist[i]; //把所有点加和
        cout << res << endl;
    }
    return 0;
}
```
