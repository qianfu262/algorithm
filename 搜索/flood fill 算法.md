# flood fill 算法

## 算法概述

flood fill 算法,主要用于找到一张图中所有的连通块,就像洪水漫遍一个水坑一样,又叫洪水填充算法。这个算法的时间复杂度是线性的.
算法的实现主要是运用bfs,宽度优先遍历

---

下面给出一道例题:

## 例题
农夫约翰有一片 $N∗M$
的矩形土地。

最近，由于降雨的原因，部分土地被水淹没了。

现在用一个字符矩阵来表示他的土地。

每个单元格内，如果包含雨水，则用”W”表示，如果不含雨水，则用”.”表示。

现在，约翰想知道他的土地中形成了多少片池塘。

每组相连的积水单元格集合可以看作是一片池塘。

每个单元格视为与其上、下、左、右、左上、右上、左下、右下八个邻近单元格相连。

请你输出共有多少片池塘，即矩阵中共有多少片相连的”W”块。

#### 样例

```
输入:
10 12
W........WW.
.WWW.....WWW
....WW...WW.
.........WW.
.........W..
..W......W..
.W.W.....WW.
W.W.W.....W.
.W.W......W.
..W.......W.

输出:
3
```
---
思路:flood fill算法,直接对整张图每一个点都进行一遍bfs,找到他所在的这个连通块的所有点,并且打上标记.下次循环到他们,就不会进入bfs中.这样就可以找到所有的连通块.



```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1010, M = N * N;
typedef pair<int, int> pii;
#define x first
#define y second
int n, m;
char g[N][N];
pii q[M];
bool st[N][N];
void bfs(int sx, int sy)
{
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    st[sx][sy] = true;
    while (hh <= tt)
    {
        pii t = q[hh++];
        for (int i = t.x - 1; i <= t.x + 1; i++)
        {
            for (int j = t.y - 1; j <= t.y + 1; j++)
            {
                if (i == t.x && j == t.y)//原来的点
                    continue;
                if (i < 0 || i >= n || j < 0 || j >= m)//判断边界
                    continue;
                if (st[i][j] || g[i][j] == '.')//已经遍历过了或者是陆地
                    continue;
                q[++tt] = {i, j};
                st[i][j] = true;
            }
        }
    } 
}
int main()
{
    // flood fill 算法
    cin >> n >> m;
    for (int i = 0; i < n; i++)
        cin >> g[i];
    int cnt = 0;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
            if (!st[i][j] && g[i][j] == 'W')
            {
                bfs(i, j);
                cnt++;
            }
    cout << cnt << endl;
    return 0;
}
```
