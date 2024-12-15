# 双向bfs 

## 算法思想

一次bfs,从起点到终点,有很多条路径,但是我们可以从起点和终点同时开始搜索这样就可以省去很多不必要的搜索,从而提高效率
算法的实现,在于开一个从起点开始的队列,再开一个从终点开始的队列,谁的元素少就先搜谁的,直到搜到重合点,就完成了搜索
而此时我们需要一个哈希表(map)存储每一个状态到起点或终点的步数,这样就可以求出最短路径了


---

## 例题

# [NOIP2002 提高组] 字串变换



## 题目描述

已知有两个字串 $A,B$ 及一组字串变换的规则（至多 $6$ 个规则），形如：

-  $A_1\to B_1$ 。
-  $A_2\to B_2$ 。

规则的含义为：在 $A$ 中的子串 $A_1$ 可以变换为 $ B_1$ ， $A_2$ 可以变换为 $B_2\cdots$ 。

例如：$A=\texttt{abcd}$ ，$B＝\texttt{xyz}$ ，

变换规则为：

-  $\texttt{abc}\rightarrow\texttt{xu}$ ，$\texttt{ud}\rightarrow\texttt{y}$ ，$\texttt{y}\rightarrow\texttt{yz}$ 。

则此时，$A$ 可以经过一系列的变换变为 $B$ ，其变换的过程为：

-  $\texttt{abcd}\rightarrow\texttt{xud}\rightarrow\texttt{xy}\rightarrow\texttt{xyz}$ 。

共进行了 $3$ 次变换，使得 $A$ 变换为 $B$ 。

## 输入格式

第一行有两个字符串 $A,B$ 。

接下来若干行，每行有两个字符串 $A_i,B_i$ ，表示一条变换规则。

## 输出格式

若在 $10$ 步（包含 $10$ 步）以内能将 $A$ 变换为 $B$ ，则输出最少的变换步数；否则输出 `NO ANSWER!`。

## 样例 #1

### 样例输入 #1

```
abcd xyz
abc xu
ud y
y yz
```

### 样例输出 #1

```
3
```

## 提示

对于 $100\%$ 数据，保证所有字符串长度的上限为 $20$ 。

**【题目来源】**

NOIP 2002 提高组第二题

---

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 6;
int n;
string a[N], b[N];
int extend(queue<string> &q, unordered_map<string, int> &da, unordered_map<string, int> &db, string a[], string b[])
//第一个参数起点或终点的队列,第二个是记录从起点出发的状态,第二个是记录从终点出发的状态,第三个是起点或终点的字符串,第四个是起点或终点的字符串
{
    string t = q.front();
    q.pop();
    for (int i = 0; i < (int)t.size(); i++)
    {
        for (int j = 0; j < n; j++)
        {
            if (t.substr(i, a[j].size()) == a[j])
            {
                string state = t.substr(0, i) + b[j] + t.substr(i + a[j].size());//substr提取子串
                if (db.count(state))
                    return da[t] + 1 + db[state];
                if (da.count(state))
                    continue;
                da[state] = da[t] + 1;
                q.push(state);
            }
        }
    }
    return 11;
}
int bfs(string A, string B)
{
    if (A == B)
        return 0;
    queue<string> qa, qb;//从起点开始搜索和从终点开始搜索
    unordered_map<string, int> da, db;//到终点和到起点的距离
    qa.push(A), da[A] = 0;
    qb.push(B), db[B] = 0;
    while (qa.size() && qb.size())
    {
        int t;
        if (qa.size() <= qb.size())//谁小就扩展谁
            t = extend(qa, da, db, a, b);
        else
            t = extend(qb, db, da, b, a);//搜索终点则需要反向   
        if (t <= 10)
            return t;//找到方法
    }
    return 11;
}

int main()
{
    /*
    子串变换
    从起点和终点同时开始搜索,双向广搜
    */
    string A, B;
    cin >> A >> B;
    while (cin >> a[n] >> b[n])
        n++;
    int step = bfs(A, B);
    if (step > 10)
        puts("NO ANSWER!");
    else
        cout << step << endl;
    return 0;
}
```