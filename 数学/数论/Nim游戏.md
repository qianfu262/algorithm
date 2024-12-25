# Nim游戏

有n堆石子 ，第i堆有a[i]个石子,两个人轮流操作，每次操作可以选择一堆石子，取走任意个石子，不能不取，取走最后一个石子的人获胜
定理:a[1] ^ a[2] ^ a[3] ^ ... ^ a[n] = 0 则先手必败，否则先手必胜
proof:
a[1] ^ a[2] ^ a[3] ^ ... ^ a[n] = x ,x中最高一位1在第k位,a[1]~a[n]中存在一个数y,第k位为0,就会有a[i]^x<a[i],所以可以拿走a[i]-(a[i]^x)个石子，使得a[1]^a[2]^a[3]^...^a[n] = 0
如果a[1] ^ a[2] ^ a[3] ^ ... ^ a[n] = 0 ,证明无论怎么拿,都不会为0,所以后手必胜
假设存在一个操作是的a1[1] ^ a1[2] ^ a1[3] ^ ... ^ a1[n] = 0
因为操作只会改变一个堆的个数,其他的两两对应,异或后都是0,



```cpp
#include <bits/stdc++.h>
using namespace std;
int main()
{
    // gameTheoryNim
    int n;
    cin >> n;
    int res = 0;
    while (n--)
    {
        int x;
        cin >> x;
        res ^= x;
    }
    if (res == 0)
        cout << "YES" << endl; // 先手必败
    else
        cout << "NO" << endl; // 先手必胜
}
```