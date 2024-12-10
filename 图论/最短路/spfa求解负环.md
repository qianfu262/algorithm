# spfa求解负环

## 算法思想

统计每个点的最短路的边的个数,如果某个点的最短路边的个数大于等于n,说明存在负环
道理很显然,n个点n条边肯定存在环,又根据最短路的定义,中间一定存在一个负环.而且spfa也可以求解正环,这两个问题显然是等价的
### 小trick
将队列改为栈,这样就可以判断是否存在负环了,速度较快
或者有一个取巧的方法,记录队列的出入队次数,如果入队次数过多,很大可能存在负环,此时可以直接跳出.这个方法谨慎使用.


下面给出一个实例:
## 01分数问题
题目描述:
给定一张 L个点、P条边的有向图，每个点都有一个权值 f[i]，每条边都有一个权值 t[i]。求图中的一个环，使“环上各点的权值之和”除以“环上各边的权值之和”最大。输出这个最大值。注意：数据保证至少存在一个环

思路:
二分加spfa求负环,对于一个环可以把点权放到边权上,然后跑spfa,如果存在正环,就说明这个环是满足条件的,然后二分答案,最后输出答案即可.(f为点权,t为边权)
fi之和/ti这和>mid等价于(fi-mid\*ti)之和>0等价于把点权放到边权上(比如放到出边上,边权就会变为f-mid*t),然后跑spfa,如果存在正环,就说明这个环是满足条件的(这个mid符合条件).
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1010, M = 5010;
int n, m;
int wf[N];
int h[N], e[N], wt[N], ne[N], idx;
double dist[N];
int cnt[N];
bool st[N];
void add(int a, int b, int c)
{
    e[idx] = b, wt[idx] = c, ne[idx] = h[a], h[a] = idx++;
}
bool check(double mid)
{
    memset(st, 0, sizeof st);
    memset(cnt, 0, sizeof cnt);
    memset(dist, 0, sizeof dist); // 其实dist初始化是多少都没关系
    queue<int> q;
    for (int i = 1; i <= n; i++)
    {
        q.push(i);
        st[i] = true;
    }//要将每个点都入队,防止图不连通,负环遍历不到
    while (!q.empty())
    {
        int t = q.front();
        q.pop();
        st[t] = false;
        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] < dist[t] + wf[t] - mid * wt[i])
            {
                // 转化成求最长路的过程
                //(wf[t] - mid * wt[i])就是边权
                dist[j] = dist[t] + wf[t] - mid * wt[i];
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n)
                    return true;
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }//判断负环的时候,可以把队列改成栈,这样就可以判断是否存在负环了,速度较快
    return false;
}
int main()
{
    /*
    判断负环的经验方法
    如果入队次数大于一个很大的数,说明很可能存在负环
   */
    cin >> n >> m;
    memset(h, -1, sizeof h);
    for (int i = 1; i <= n; i++)
        cin >> wf[i];
    for (int i = 0; i < m; i++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    double l = 0, r = 1000;
    while (r - l > 1e-4)
    {
        double mid = (l + r) / 2;
        if (check(mid))
            l = mid;
        else
            r = mid;
    }
    printf("%.2lf\n", l);
    return 0;
}
```