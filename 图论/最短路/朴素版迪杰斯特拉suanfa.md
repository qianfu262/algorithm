# 朴素版迪杰斯特拉算法模板

迪杰斯特拉算法可以用于求解单源最短路. 当边权为正或者不存在负环时,迪杰斯特拉算法就可以使用,时间复杂度为o( $n^2$ ),n为点数
## 算法思路
算法的思路在于,迭代n次,每一次迭代先找到距离最短的点,然后根据这个点到其他点的距离更新其他点,这样迭代n次就可以找到最短路径.
由于是稠密图,邻接矩阵存储效率高,所以使用邻接矩阵存储图
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 510;
int n, m;    // 点数和边数
int g[N][N]; // 邻接矩阵
int dist[N]; // dist表示从起点到每个点的最短距离
bool st[N];  // 是否已经在我们要找的路径之中
int dijkstra()
{      
    memset(dist, 0x3f, sizeof dist); // 初始化为无穷大
    dist[1] = 0;                     // 起点距离为0
    for (int i = 0; i < n - 1; i++)  // 循环n-1次
    {
        int t = -1;
        for (int j = 1; j <= n; j++)
        {
            if (!st[j] && (t == -1 || dist[j] < dist[t]))
                t = j;//找到距离最短的点
        }
        st[t] = true;//标记已经走过，下一轮不再计算
        
        for (int j = 1; j <= n;j++)
            dist[j] = min(dist[j], (dist[t] + g[t][j]));//更新最短距离
    }//例如第一个处理的点就是一号点，然后可以找到一号点到其他点的最短距离，然后处理二号点，找到二号点到其他点的最短距离，以此类推，直到处理完所有点
    if (dist[n] == 0x3f3f3f3f)
        return -1;
    return dist[n];
}
int main()
{
    scanf("%d%d", &n, &m);
    memset(g, 0x3f, sizeof g); // 初始化为无穷大
    while (m--)
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);//c表示边的权值
        g[a][b] = min(g[a][b], c); // 找的是最短路,所以存储最小的边权
    }
    int res = dijkstra();
    printf("%d\n", res);
}
```
