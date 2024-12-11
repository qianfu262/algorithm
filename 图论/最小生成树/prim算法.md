# 最小生成树 prim算法

## 最小生成树
最小生成树是指，在一个给定的无向图中，找到一个子图，使得这个子图包含所有的顶点，并且边的权值之和最小。
prim算法,时间复杂度O( $n^2$ ),适用于稀疏图.
算法思路:这个算法类似于Dijkstra算法,每次选择距离集合最近的点,加入到集合中,并更新集合外其它点到集合的距离. 先迭代n次,每一次迭代,找到距离连通块最近的点,然后把这个点加入到连通块当中,并更新集合外其它点到集合的距离. 最后累加所有边的权值,就是最小生成树的权值.注意需要先累加,再更新集合,否则如果存在负权自环,会出错.


```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 5100;
int n, m;
int g[N][N]; 
int dist[N]; // 记录每个点到集合的距离
bool st[N];  // 记录每个点是否在集合中
int prim()
{
    memset(dist, 0x3f, sizeof dist); // 初始化
    int res = 0;
    for (int i = 0; i < n; i++) // n次迭代找到n个点
    {
        int t = -1;
        for (int j = 1; j <= n; j++)
        {
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
            {
                t = j;
            }
        }
        if (i != 0 && dist[t] == 0x3f3f3f3f)
            return 0x3f3f3f3f; // 最短距离是正无穷，表明无法连通
        if (i != 0)
            res += dist[t];
        // 要先累加，再更新集合，否则如果存在负权自环，会出错
        for (int j = 1; j <= n; j++)
            dist[j] = min(dist[j], g[t][j]); // 更新距离
        st[t] = true; // 计入到集合中
    }
    return res;
}
int main()
{
    scanf("%d%d", &n, &m);
    memset(g, 0x3f, sizeof g); // 初始化
    while (m--)
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = g[b][a] = min(g[a][b], c); // 无向图
    }
    int ans = prim();
    if (ans == 0x3f3f3f3f)
        puts("orz");
    else
        printf("%d\n", ans);
    return 0;
}
```