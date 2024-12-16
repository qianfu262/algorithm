# dfs 迭代加深

## 概念
迭代加深主要针对的是某些题目,要搜索的层数其实很多,但是答案很有可能在很浅的层就可以被找到,此时我们可以由小到大枚举我们搜索的深度,当某些时候搜索层数超过最大搜索层数就是,直接结束搜索,这样就可以避免搜索很多不必要的层数,节省时间。



## 例题
满足如下条件的序列 X（序列中元素被标号为 1、2、3…m）被称为“加成序列”：

1. $X[1]=1$
2. $X[m]=n$
3. $X[1]<X[2]<…<X[m−1]<X[m]$
4. 对于每个 k（ $2 \leq k \leq m$ ）都存在两个整数 i 和 j （ $1 \leq i,j \leq k−1$ ，i 和 j 可相等），使得 $X[k]=X[i]+X[j]$ 。

你的任务是：给定一个整数 n，找出符合上述条件的长度 m 最小的“加成序列”。

如果有多个满足要求的答案，只需要找出任意一个可行解。

#### 输入格式

输入包含多组测试用例。

每组测试用例占据一行，包含一个整数 n。

当输入为单行的 0 时，表示输入结束。

#### 输出格式

对于每个测试用例，输出一个满足需求的整数序列，数字之间用空格隔开。

每个输出占一行。

#### 数据范围

$1 \leq n \leq 100$

#### 输入样例：

```
5
7
12
15
77
0
```

#### 输出样例：

```
1 2 4 5
1 2 4 6 7
1 2 4 8 12
1 2 4 5 10 15
1 2 4 8 9 17 34 68 77
```

---


```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 110;
int n;
int path[N];
bool dfs(int u, int depth) // u表示当前层数,depth表示最大深度
{
    if (u > depth)
        return false;
    if (path[u - 1]==n)
        return true;
    bool st[N] = {0};
    for (int i = u - 1; i >= 0; i--)
    {
        for (int j = i; j >= 0; j--)//由于是组合数顺序,要从后往前枚举
        {
            int s = path[i] + path[j];
            if (s > n || s <= path[u - 1] || st[s])//第一个是超出n的范围,第二个是违反递增条件,第三个是重复的
                continue;
            st[s] = true;//只是对于每一层记录这个数是否被用过,进行的dfs剪枝,从而不用恢复现场.
            path[u] = s;
            if (dfs(u + 1, depth))
                return true;
        }
    }
    return false;
}
int main()
{
    // 迭代加深
    path[0] = 1;
    while (cin >> n, n)
    {
        int depth = 1;
        while (!dfs(1, depth))
            depth++;
        for (int i = 0; i < depth; i++)
            cout << path[i] << " ";
        cout << endl;
    }
}

```