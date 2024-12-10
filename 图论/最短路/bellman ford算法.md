# bellman-ford 算法

## 算法思想
bellman-ford 算法是一种求解单源最短路径的算法，适用于边权为负数的图.
首先用结构体存储每一条边,然后迭代n次,然后边遍历所有边，并且用上一次迭代的结果更新dist数组，防止出现串联(指的是一条边可以到达另一个点，然后另一个点又可以到达另一个点，这样就会导致串联，所以需要用上一次迭代的结果更新dist数组).
然后判断路径不存在的情况,只需要大于一个很大的数,因为负权边会让无穷大变小,所以判断是否大于一个很大的数即可.
## 原版bellman-ford算法
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 501, M = 10100;
int n, m;
int dist[N], backup[N];
struct Edge
{
    int a, b, w;
} edge[M]; // 储存边的信息
int bellman_ford()
{
    memset(dist, 0x3f, sizeof dist); // 初始化dist数组，表示距离
    dist[1] = 0;                     // 起点距离为0
    for (int i = 0; i < n; i++)
    {
        memcpy(backup, dist, sizeof dist); // 备份操作，存储上一次的结果，
        // 把dist数组的内容复制到backup数组中
        for (int j = 0; j < m; j++)
        {
            int a = edge[j].a, b = edge[j].b, w = edge[j].w;
            dist[b] = min(dist[b], backup[a] + w); // 用上次迭代的结果更新dist数组，防止出现串联
        }
        if (dist[n] > 0x3f3f3f3f / 2) // 如果dist[n]大于一个很大的数，说明无法到达
            return -1; // 存在负权边，会让无穷大变小，所以判断是否大于一个很大的数
        else
            return dist[n];
    }
}
int main()
{
    scanf("%d%d%d", &n, &m);
    // k表示最多不能超过k条边
    /*bellman ford 算法*/
    for (int i = 0; i < m; i++)
        scanf("%d%d%d", &edge[i].a, &edge[i].b, &edge[i].w); // 读入边的数据
    int t = bellman_ford();
    if (t == -1)
        printf("impossible\n");
    else
        printf("%d\n", t);
    return 0;
}
```

## bellman-ford算法其他用处
用法:求解只有k条边的最短路
只需要把迭代次数变为k次即可

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 501, M = 10100;
int n, m, k;
int dist[N], backup[N];
struct Edge
{
    int a, b, w;
} edge[M];
int bellman_ford()
{
    memset(dist, 0x3f, sizeof dist); // 初始化dist数组，表示距离
    dist[1] = 0;                     // 起点距离为0
    // 因为不能超过k条边，所以迭代k次
    for (int i = 0; i < k; i++)
    {
        memcpy(backup, dist, sizeof dist); // 备份操作，存储上一次的结果，
        for (int j = 0; j < m; j++)
        {
            int a = edge[j].a, b = edge[j].b, w = edge[j].w;
            dist[b] = min(dist[b], backup[a] + w); // 用上次迭代的结果更新dist数组，防止出现串联
        }
        if (dist[n] > 0x3f3f3f3f / 2) // 如果dist[n]大于一个很大的数，说明无法到达
            return -1; // 存在负权边，会让无穷大变小，所以判断是否大于一个很大的数
        else
            return dist[n];
    }
}
int main()
{
    scanf("%d%d%d", &n, &m, &k);
    // k表示最多不能超过k条边
    for (int i = 0; i < m; i++)
        scanf("%d%d%d", &edge[i].a, &edge[i].b, &edge[i].w); 

    int t = bellman_ford();
    if (t == -1)
        printf("impossible\n");
    else
        printf("%d\n", t);
    return 0;
}
```