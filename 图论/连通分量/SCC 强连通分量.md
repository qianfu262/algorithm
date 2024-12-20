# SCC强双连通分量

强连通分量是对于一个有向图,连通分量,对于分量中任意两点u,v,必然可以从u走到v
求出强连通分量之后,可以将每个强连通分量缩成一个点,形成一个新的图,这个图是有向无环图,可以用拓补排序求出拓扑序


tarjan算法求强连通分量
```cpp
#include <bits/stdc++.h>
using namespace std;
// stronglyConnectedComponents
const int N = 1e5 + 10;
int h[N], e[N], ne[N], idx;            // 建图
int dfn[N], low[N], stk[N], in_stk[N]; // 第一个是时间戳,第二个是可以访问到的最小时间戳,第三个是栈,第四个是是否在栈中
int n, m;
int timestamp, top, scc_cnt, id[N], cnt[N]; // scc_cnt是强连通分量个数,id是每个点属于哪个强连通分量
int dout[N];                                // 缩点之后记录每个点的出度,用于拓补排序
void add(int a, int b)
{
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx++;
}
void tarjan(int u)
{
    // 进入u点时,盖戳,入栈
    dfn[u] = low[u] = ++timestamp; // 时间戳
    stk[++top] = u;
    in_stk[u] = true; // 在栈中,打上标记
    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!dfn[j]) // 如果j点没有被访问过
        {
            tarjan(j); // 访问,盖戳,入栈
            low[u] = min(low[u], low[j]);
        }
        else if (in_stk[j]) // 如果j点在栈中且已经访问
        {
            low[u] = min(low[u], dfn[j]);
        }
    }
    // 记录Scc
    if (dfn[u] == low[u]) // u是强连通分量的最高点
    {
        // 找到一个强连通分量
        int y;
        ++scc_cnt; // 强连通分量个数加1
        do
        {
            y = stk[top--];
            in_stk[y] = false;
            id[y] = scc_cnt; // 标记这个点属于哪个强连通分量
            cnt[scc_cnt]++;  // 强连通分量中点的个数
        } while (y != u);
    }
}
int main()
{
    // 缩点
    cin >> n >> m;
    memset(h, -1, sizeof h);
    while (m--)
    {
        int a, b;
        cin >> a >> b;
        add(a, b);
    }
    for (int i = 1; i <= n; i++)
    {
        if (!dfn[i])//如果i点没有被访问过
            tarjan(i);//因为不知道哪个点可以走到其它点,所以从1到n遍历
            //但是,如果有一个超级原点可以走到其它点,那么只需要从超级原点开始遍历    
    }
    for (int i = 1; i <= n; i++)
    {
        for (int j = h[i]; j != -1; j = ne[j])
        {
            int k = e[j];
            int a = id[i], b = id[k];
            if (a != b) // 不同连通分量之间
                dout[a]++;
        }
    }
    int zero = 0, sum = 0;
    for (int i = 1; i <= scc_cnt; i++)
        if (dout[i] == 0)
        {
            zero++;
            sum += cnt[i];
            if (zero >= 2)
            {
                cout << 0;
                return 0;
            }
        }
    cout << sum;
    return 0;
}
```