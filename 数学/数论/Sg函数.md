# 博弈论 SG函数

mex运算mex(S)表示不属于集合S的最小非负整数
SG函数 在有向图游戏中,对于每一个节点x,设从x出发到达节点y1,y2,...,yk
则SG(x)为y1,y2,...,yk的SG函数值构成的集合再进行mex运算
sg(x)=0,下一步一定也是0,否则sg(x)就不是0了
sg(x)!=0,下一步肯定不是0,否则sg(x)就不是0了

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=110,M=100010;
int n, m;
int s[N],f[N];
int sg(int x)
{
    if(f[x]!=-1)
    {
        return f[x];//如果f[x]已经计算过,直接返回
    }
    unordered_set<int> S;
    for (int i = 0; i < m; i++)
    {
        int sum=s[i];
        if (x >=sum)
        {
            S.insert(sg(x - s[i]));
        }
    }
    for (int i = 0;; i++)
    {
        if (!S.count(i))//如果i不在S集合中
        {
            f[x] = i;
            return i;
        }
    }
}
int main()
{
    //求SG函数
    cin >> m;
    for (int i = 0; i < m; i++)
        cin >> s[i];
    cin >> n;
    memset(f, -1, sizeof(f));
    int res = 0;
    for (int i = 0; i < n; i++)
    {
        int x;
        cin >> x;
        res ^= sg(x);
    }
    if (res)
    {
        cout << "Yes" << endl;
    }
    else
    {
        cout << "No" << endl;
    }
    return 0;
}
```