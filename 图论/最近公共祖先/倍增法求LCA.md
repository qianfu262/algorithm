# 倍增法求LCA

预处理复杂度是 $o(nlogn)$ ,每次查询的复杂度是 $o(logn)$


```cpp
#include <bits/stdc++.h>
using namespace std;
// LCA
/*
倍增法
fa[i][j]表示i向上跳2^j步的祖先.0<=j<=log2(n)
depth[i]表示i的深度
哨兵:如果i向上跳2^j步的祖先不存在,则fa[i][j]=0,depth[0]=0
步骤:
1.预处理fa和depth数组
2.对于每次查询,先让两个节点跳到同一层
3.然后让两个节点同时向上跳,一直跳到它们最近公共祖先的下一层
*/
const int N = 40010, M = N * 2;
int n, m;
int h[N], e[M], ne[M], idx;
int depth[N], fa[N][16];
int q[N];
void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}
void bfs(int root)//预处理两个数组
{
    memset(depth, 0x3f, sizeof depth);
    depth[0] = 0, depth[root] = 1;//哨兵任何跳不到的点都会归结为0,然后深度为零
    int hh = 0, tt =0;
    q[0] = root;
    while (hh <= tt)//bfs求深度
    {
        int t = q[hh++];
        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (depth[j] > depth[t] + 1)
            {
                depth[j] = depth[t] + 1;
                q[++tt] = j;
                fa[j][0] = t;
                for (int k = 1; k <= 15; k++)
                    fa[j][k] = fa[fa[j][k - 1]][k - 1]; //;连续跳两次2^(k-1)步可以跳到2^k步
            }
        }
    }
}
int lca(int a, int b)
{
    if (depth[a] < depth[b])
        swap(a, b);//深度大的节点先跳
    for (int k = 15;k >= 0; k--)
        if (depth[fa[a][k]] >= depth[b])//本质是让用二进制凑出depth[a]-depth[b]
            a = fa[a][k];//让a跳到和b同一层
    if (a == b)//如果a和b已经同一层了,那么a就是它们的最近公共祖先
        return a;
    for (int k = 15; k >= 0; k--)//让a和b同时向上跳,直到跳到它们的最近公共祖先的下一层
        if (fa[a][k] != fa[b][k])
        {
            //没有跳到公共祖先上就继续跳
            //也是用二进制凑出跳到公共祖先的下一层
            a = fa[a][k];
            b = fa[b][k];
        }
    return fa[a][0];
}
int main()
{
    cin >> n;
    int root ;
    memset(h, -1, sizeof h);
    for (int i = 0; i < n; i++)
    {
        int a, b;
        cin >> a >> b;
        if (b == -1)
            root = a;
        add(a, b), add(b, a);
    }
    bfs(root);
    cin >> m;
    while (m--)
    {
        int a, b;
        cin >> a >> b;
        int p = lca(a, b);
        if (p == a)
            cout << 1 << endl;
        else if (p == b)
            cout << 2 << endl;
        else
            cout << 0 << endl;
    }
    return 0;
}
```