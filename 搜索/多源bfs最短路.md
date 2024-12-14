# 多源bfs最短路问题

## 算法思路

首先bfs可以求边权为1的最短路，而多源bfs就是将所有起点都加入队列，然后进行bfs，这样就可以求出所有点到其他点的最短路。,这样可以避免使用floyd算法，算法时间复杂度为O( $n^2$ )。

---

## 题目描述

给定一个 N 行 M 列的 01 矩阵 A，A\[i]\[j] 与 A\[k]\[l] 之间的曼哈顿距离定义为：

dist(i,j,k,l)=|i−k|+|j−l|

输出一个 N 行 M 列的整数矩阵 B，其中：

$B[i][j]=dist(i,j,k,l)的最小值$

#### 输入格式

第一行两个整数 N,M。

接下来一个 N 行 M 列的 01 矩阵，数字之间没有空格。

#### 输出格式

一个 N 行 M 列的矩阵 B，相邻两个整数之间用一个空格隔开。

#### 数据范围

$1\leq N,M \leq 1000$

#### 输入样例：

```
3 4
0001
0011
0110
```

#### 输出样例：

```
3 2 1 0
2 1 0 0
1 0 0 1
```
---

思路:第一次bfs,把为1的点加入队列之中,然后开始扩展出距离为1的点,一层层扩展,从而求出多源最短路

---
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef pair<int, int> pii;
#define x first
#define y second
const int N = 1010, M = N * N;
int n, m;
char g[N][N];
pii q[M];
int dist[N][N];//将dist与st数组功能合并
void bfs()
{
    memset(dist, -1, sizeof(dist));
    int hh = 0, tt = 0;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
            if (g[i][j] == '1')
            {
                dist[i][j] = 0;
                q[tt++] = {i, j};
            }
    int dx[] = {0, 0, 1, -1}, dy[] = {1, -1, 0, 0};
    while (hh <= tt)
    {
        auto t = q[hh++];
        for (int i = 0; i < 4; i++)
        {
            int a = t.x + dx[i], b = t.y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= m)
                continue;
            if (dist[a][b] != -1)
                continue;
            dist[a][b] = dist[t.x][t.y] + 1;//计算距离
            q[tt++] = {a, b};
        }
    }
}
int main()
{
    // 多源Bfs
    cin >> n >> m;
    for (int i = 0; i < n; i++)
        cin >> g[i];
    bfs();
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
            cout << dist[i][j] << ' ';
        cout << endl;
    }
    return 0;
}
```

