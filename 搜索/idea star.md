# idea* 算法

## 算法思想
其本质上也和bfs的A*算法类似,都是通过一个估价函数来剪枝,从而减少搜索的次数,
直接看一道例题.

---

## 例题
给定 n 本书，编号为 1∼n。

在初始状态下，书是任意排列的。

在每一次操作中，可以抽取其中连续的一段，再把这段插入到其他某个位置。

我们的目标状态是把书按照 $1∼n$ 的顺序依次排列。

求最少需要多少次操作。

#### 输入格式

第一行包含整数 T，表示共有 T 组测试数据。

每组数据包含两行，第一行为整数 n，表示书的数量。

第二行为 n 个整数，表示 $1∼n$ 的一种任意排列。

同行数之间用空格隔开。

#### 输出格式

每组数据输出一个最少操作次数。

如果最少操作次数大于或等于 5 次，则输出 `5 or more`。

每个结果占一行。

#### 数据范围

$1 \leq n \leq 15$

#### 输入样例：

```
3
6
1 3 4 6 2 5
5
5 4 3 2 1
10
6 8 5 3 4 7 2 9 1 10
```

#### 输出样例：

```
2
3
5 or more
```
---
这道题的关键在于如何找到估价函数,即如何估计当前这个排列需要多少次操作才能变成目标排列.此时我们要注意到后继对这一信息,一个排好的序列,相邻两个元素之间一定满足后继关系,即 $a_{i+1}=a_i+1$ ,此时我们统计错误的后继的数量,我们发现每一次操作最多可以改变三个数的后继关系,每次最多减少三,那么至少的操作次数就是错误的数量除以三,向上取整,这就是我们的估价函数.


----
## 代码



```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 15;
int n;
int q[N];//存储结果
int w[5][N];
int f()//估价函数,存储后继对不正确的数量
{
    int tot = 0;
    for (int i = 0; i + 1 < n; i++)
        if (q[i + 1] != q[i] + 1)
            tot++;
    return (tot + 2) / 3;
}
bool dfs(int u, int depth)
{
    if (u + f() > depth)//启发函数超过最大深度
        return false;
    if (f() == 0)
        return true;
    for (int len = 1; len <= n; len++)
        for (int l = 0; l + len - 1 < n; l++)
        {
            int r = l + len - 1;
            for (int k = r + 1; k < n; k++)
            {
                memcpy(w[u], q, sizeof(q)); // 拷贝数组
                int y = l;
                for (int x = r + 1; x <= k; x++, y++)
                    q[y] = w[u][x];
                for (int x = l; x <= r; x++, y++)//将l到r的元素移到k+1的位置
                    q[y] = w[u][x];
                if (dfs(u + 1, depth))
                    return true;
                memcpy(q, w[u], sizeof(q)); // 恢复现场
            }
        }
    return false;
}
int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        cin >> n;
        for (int i = 0; i < n; i++)
            cin >> q[i];
        int depth = 0;
        while (!dfs(0, depth)&&depth<5)
            depth++;
        if (depth >= 5)
            cout << "5 or more" << endl;
        else
            cout << depth << endl;
    }
}

```