# tarjan 离线算法求LCA
算法的时间复杂度是 $o(n+m)$

---
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef pair<int, int> pii;
const int N = 20010, M = 2 * N;
int n, m;
int h[N], e[M], w[M], ne[M], idx;
int p[N];
int res[N];
int st[N];
int dist[N];//处理一下和一号点之间的距离
vector<pii> query[N]; // first查询的另外一个点,second存查询编号
void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}
void dfs(int u, int fa)
{
    for (int i = h[u]; i!=-1; i = ne[i])
    {
        int j = e[i];
        if (j == fa)
            continue;
        dist[j] = dist[u] + w[i];
        dfs(j, u);//深搜求距离
    }
}
int find(int x)
{
    if (x != p[x])
        p[x] = find(p[x]);
    return p[x];
}
void tarjan(int u)
{
    st[u] = 1;//标记为正在搜索的分支
    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!st[j])//j还没有被遍历过,是第三类的点
        {
            //上面是回溯之前,所以标记为1
            tarjan(j);//遍历所有子节点,找到它们的最近公共祖先
            p[j] = u;//合并到一个集合中,j的最近公共祖先就是u
            //回溯的时候把儿子指向父亲
        }
    }
    for (auto x : query[u])//枚举以u为起点的所有查询
    {
        int y= x.first, id = x.second;//需要查询的点,查询编号
        if (st[y]==2)//先找的会先压缩,不影响后来的结果
        {
            int anc = find(y);//集合编号就是y的最近公共祖先
            res[id] = dist[u] + dist[y] - 2 * dist[anc];
            //查询树上两个点之间的距离,就是两个点到根的距离之和减去两倍根到最近公共祖先的距离
        }
    }
    st[u] = 2;//回溯的时候标记一下
    //回溯完成了,标记为2
}
int main()
{
    // LCA
    /*
    深度优先遍历的过程中,把结点分为三类
    第一类:已经遍历过的结点且已经回溯过的结点,对应2
    第二类:正在搜索的分支,对应1
    第三类:还未搜索的分支,对应0
    */
    cin >> n >> m;
    memset(h, -1, sizeof h);
    for (int i = 1; i <=n - 1; i++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
        add(b, a, c);
    }
    for (int i = 0; i < m; i++)
    {
        int a, b;
        cin >> a >> b;
        if (a != b)
        {
            query[a].push_back({b, i});//存查询编号,从a到b,查询编号为i
            query[b].push_back({a, i});
        }
    }
   
    for (int i = 1; i <= n; i++)
      p[i] = i;
    dfs(1,-1);
    tarjan(1);
    for (int i = 0; i < m; i++)
    {
        cout << res[i] << endl;
    }
    return 0;
}

```