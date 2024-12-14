# A*算法

## A*算法

主要用于优化bfs问题,当计算量特别大是,A*算法可以减少遍历那些不必要的量.
使用A*算法的前提是:题目一定要保证有解
核心思想是,用一个估值函数让我们优先搜到某些点,从而减少搜索量.
而算法要求成立的条件是:估值函数<=真实距离
**注意**:A*不能保证除了终点之外的点第一次出队就是最优解,而且也不能保证每个点只能出队一次
状态state
dstate:起点到当前点的真实状态
gstate:终点到当前点的真实状态
fstate:当前点到终点的估计状态
估计的总长度=dstate+fstate
估计距离:dstate+fstate
成立条件：fstate<=gstate
而且题目一定要保证有解
算法实现需要小根堆,每一次进入队列时加入的是fstate+dstate,每次出队都是fstate+dstate最小的,知道找到终点为止,会减少很多计算量.

---

## 题目描述

在一个3×3的网格中，1~8这8个数字和一个“x”恰好不重不漏地分布在这3×3的网格中。

例如：

```
1 2 3
x 4 6
7 5 8
```

在游戏过程中，可以把“x”与其上、下、左、右四个方向之一的数字交换（如果存在）。

我们的目的是通过交换，使得网格变为如下排列（称为正确排列）：

```
1 2 3
4 5 6
7 8 x
```

例如，示例中图形就可以通过让“x”先后与右、下、右三个方向的数字交换成功得到正确排列。

交换过程如下：

```
1 2 3   1 2 3   1 2 3   1 2 3
x 4 6   4 x 6   4 5 6   4 5 6
7 5 8   7 5 8   7 x 8   7 8 x
```

现在，给你一个初始网格，请你求出得到正确排列至少需要进行多少次交换。

输入格式\
输入占一行，将3×3的初始网格描绘出来。

例如，如果初始网格如下所示：

```
1 2 3

x 4 6

7 5 8
```

则输入为：`1 2 3 x 4 6 7 5 8`

输出格式\
输出占一行，包含一个整数，表示最少交换次数。

如果不存在解决方案，则输出”-1”。

#### 样例

```
输入样例：
2  3  4  1  5  x  7  6  8 
输出样例
19
```
---
思路:本题是八数码的加强版做法,
而八数码有解的充要条件是逆序对的个数是偶数,所以当逆序对是奇数时,直接输出-1,从而可以判定有解情况.
而估计函数,我们取得是初始状况到最后结果,两个数位置的曼哈顿距离.

---
```cpp
#include <cstring>
#include <iostream>
#include <algorithm>
#include <unordered_map>
#include <queue>
using namespace std;
// eightDigital问题
#define x first
#define y second
typedef pair<int, string> pis;
int f(string state)//计算曼哈顿估计距离
{
    int res = 0;
    for (int i = 0; i < state.size(); i++)
        if (state[i] != 'x')
        {
            int t = state[i] - '1';
            res += abs(i/ 3 - t / 3) + abs(i % 3 - t % 3);
        }
    return res;
}
string bfs(string start)
{
    string end = "12345678x";
    char op[] = "urdl";//上右下左
    unordered_map<string, int> dist;
    unordered_map<string, pair<char, string>> prev;//记录一下前驱状态和操作
    priority_queue<pis, vector<pis>, greater<pis>> heap;//pis,第一维记录估计距离,第二维记录状态
    dist[start] = 0;
    heap.push({f(start), start});
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    while (heap.size())//开始宽搜
    {
        auto t = heap.top();
        heap.pop();

        string state = t.y;
        if (state == end)
            break;
        int x, y;
        for (int i = 0; i < 9; i++)
            if (state[i] == 'x')
            {
                x = i / 3, y = i % 3;
                break;
            }

        string source = state;//存储原状态
        for (int i = 0; i < 4; i++)
        {
            int a = x + dx[i], b = y + dy[i];
            if (a < 0 || a >= 3 || b < 0 || b >= 3)
                continue;
            state = source;
            swap(state[x * 3 + y], state[a * 3 + b]);//交换位置,完成操作
            if (dist.count(state) == 0 || dist[state] > dist[source] + 1)
            //第一个是之前没有被扩展过,或者是之前被扩展过但是距离比较大
            {
                dist[state] = dist[source] + 1;
                prev[state] = {op[i], source};
                heap.push({dist[state] + f(state), state});
            }
        }
    }
    string res;
    while (end != start)
    {
        res += prev[end].x;
        end = prev[end].y;//从终点开始倒退
    }
    reverse(res.begin(), res.end());
    return res;
}

int main()
{
    string start, seq;
    char c;
    while (cin >> c)
    {
        start += c;
        if (c != 'x')
            seq += c;
    }
    int cnt = 0;//求解逆序对数量
    for (int i = 0; i < 8; i++)
        for (int j = i; j < 8; j++)
            if (seq[i] > seq[j])
                cnt++;
                
    if (cnt & 1)
        puts("unsolvable");
    else
        cout << bfs(start) << endl;
    return 0;
}

```