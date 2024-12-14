# 双端队列bfs

## 算法思想
首先,bfs可以求边权只有0和1的最短路问题,其中可以用双端队列进行优化,思想主要是,每一次边权为0的点就加入队头,优先搜索,而边权为1的点就加入队尾,后搜索,这样可以保证边权为0的点一定在边权为1的点之前被搜索到,从而保证最短路




---

## 例题

# [BalticOI 2011 Day1] Switch the Lamp On 电路维修

## 题面翻译

### 题目描述
Casper 正在设计电路。有一种正方形的电路元件，在它的两组相对顶点中，有一组会用导线连接起来，另一组则不会。有 $N\times M$ 个这样的元件，你想将其排列成 $N$ 行，每行 $M$ 个。 电源连接到板的左上角。灯连接到板的右下角。只有在电源和灯之间有一条电线连接的情况下，灯才会亮着。为了打开灯，任何数量的电路元件都可以转动 90°（两个方向）。

![](https://cdn.luogu.com.cn/upload/pic/1286.png)

![](https://cdn.luogu.com.cn/upload/pic/1285.png)

在上面的图片中，灯是关着的。如果右边的第二列的任何一个电路元件被旋转 90°，电源和灯都会连接，灯被打开。现在请你编写一个程序，求出最小需要多少旋转多少电路元件。

### 输入输出格式

#### 输入格式
输入的第一行包含两个整数 $N$ 和 $M$ ，表示盘子的尺寸。 在以下 $N$ 行中，每一行有 $M$ 个符号 `\` 或 `/`，表示连接对应电路元件对角线的导线的方向。
#### 输出格式：
如果可以打开灯，那么输出只包含一个整数，表示最少转动电路元件的数量。

如果不可能打开灯，输出 `NO SOLUTION`。


## 样例 #1

### 样例输入 #1

```
3 5
\\/\\
\\///
/\\\\
```

### 样例输出 #1

```
1
```

## 提示

对于 $40\%$ 的数据， $1 \le N \le 4$ ， $1 \le M \le 5$ 。

对于所有数据， $1 \le N,M \le 500$ 。

```cpp
#include <bits/stdc++.h>
using namespace std;
#define x first
#define y second
typedef pair<int, int> pii;
const int N = 510, M = N * N;
int n, m;
char g[N][N];
int dist[N][N];
bool st[N][N];
int bfs()
{
    deque<pii> q;
    memset(dist, 0x3f, sizeof dist);
    memset(st, false, sizeof st);
    char cs[] = "\\/\//";//转义字符,实际上是"\/\/"
    int dx[] = {-1, -1, 1, 1}, dy[] = {-1, 1, 1, -1};
    int ix[] = {-1, -1, 0, 0}, iy[] = {-1, 0, 0, -1};//两种坐标的偏移,点坐标和方格坐标
    q.push_back({0, 0});
    dist[0][0] = 0;
    while (q.size())
    {
        auto t = q.front();
        q.pop_front();
        int x = t.x, y = t.y;
        if(x==n&&y==m)
            return dist[x][y];
        if (st[x][y])
            continue; // 算过了
        st[x][y] = true;
        for (int i = 0; i < 4; i++)
        {
            int a = x + dx[i], b = y + dy[i];
            if (a < 0 || a > n || b < 0 || b > m)
                continue;
            int ga = x + ix[i], gb = y + iy[i];
            int w = (g[ga][gb] != cs[i]);//判断边权是否为0,电路是否可以对得上
            int d=dist[x][y]+w;
            if(d<=dist[a][b])
            {
                dist[a][b]=d;
                if(w==0)
                    q.push_front({a,b});
                else
                    q.push_back({a, b});
            }
        }
    }
    return -1;//一定不会执行
}
int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        cin >> n >> m;
        for (int i = 0; i < n; i++)
            cin >> g[i];
        if ((n + m) % 2 == 1)
        {
            puts("NO,SOLUTION");
            continue;
        }
        else
            cout << bfs() << endl;
    }
    return 0;
}
```
