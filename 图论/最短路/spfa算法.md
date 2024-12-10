# spfa算法

## 算法思想
spfa本质是对bellman-ford的优化,bellman-ford每次都要遍历n条边,而spfa的思想是,每次我更新了哪一个点,我就拿它去更新另外的点,这样时间复杂度可以优化到O(km),k为常数
为了防止队列存储重复的元素,每次入队打上标记,出队去除标记

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;
int n, m;
int h[N], e[N], ne[N], w[N], idx;
int dist[N];
bool st[N];
void add(int a, int b, int c) 
{
    e[idx] = b;
    w[idx] = c;
    ne[idx] = h[a];
    h[a] = idx++;
} 
int spfa()
{
    memset(dist, 0x3f, sizeof dist); // 初始化为无穷大
    dist[1] = 0;
    queue<int> q;
    q.push(1);  
    st[1] = true; // 防止存储重复的点
    while (q.size())
    {
        int t = q.front();
        q.pop();
        st[t] = false;
        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;  
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            } // 如果更新了，就把这个点放入队列中
        }
    }
    if (dist[n] == 0x3f3f3f3f)
        return -1;
    return dist[n]; 
}
int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h); 
    while (m--)
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c); // c表示边的权值
        add(a, b, c);
    }
    int res = spfa();
    printf("%d\n", res);
    return 0;
}
```