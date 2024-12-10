# floyd算法

## 算法思想
求多源最短路,时间复杂度是o( $n^3$ )
算法代码很简单,只需要三层循环即可.

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 210;
int n, m, Q;
int d[N][N];
void floyd()
{
    for (int k = 1; k <= n; k++)
    {
        for (int i = 1; i <= n; i++)
        {
            for (int j = 1; j <= n; j++)
            {
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
            }
        }
    }
}
int main()
{
    scanf("%d%d", &n, &m, &Q);
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            if (i == j)
                d[i][j] = 0; // 删去自环
            else
                d[i][j] = 0x3f3f3f3f; // 初始化为无穷大
        }
    }
    while (m--)
    {
        int a, b, w;
        scanf("%d%d%d", &a, &b, &w);
        d[a][b] = min(d[a][b], w); // 存储边权,而且只储存最小的边权
    }
    floyd();
    while (Q--)
    {
        int a, b;
        scanf("%d%d", &a, &b);
        if (d[a][b] > 0x3f3f3f3f / 2)
            puts("No");
        else
            printf("%d\n", d[a][b]);
    }
}
```
