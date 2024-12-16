# dfs剪枝 数独

## 题目描述
数独是一种传统益智游戏，你需要把一个 9×9 的数独补充完整，使得数独中每行、每列、每个 3×3 的九宫格内数字 1∼9 均恰好出现一次。

请编写一个程序填写数独。

#### 输入格式

输入包含多组测试用例。

每个测试用例占一行，包含 81 个字符，代表数独的 81 个格内数据（顺序总体由上到下，同行由左到右）。

每个字符都是一个数字（1−9）或一个 `.`（表示尚未填充）。

您可以假设输入中的每个谜题都只有一个解决方案。

文件结尾处为包含单词 `end` 的单行，表示输入结束。

#### 输出格式

每个测试用例，输出一行数据，代表填充完全后的数独。

#### 输入样例：

```
4.....8.5.3..........7......2.....6.....8.4......1.......6.3.7.5..2.....1.4......
......52..8.4......3...9...5.1...6..2..7........3.....6...1..........7.4.......3.
end
```

#### 输出样例：

```
417369825632158947958724316825437169791586432346912758289643571573291684164875293
416837529982465371735129468571298643293746185864351297647913852359682714128574936
```

## 题目分析
本题是dfs剪枝的经典题目,首先我们需要用一串二进数代表每一行每一列,每一个九宫格的数字填写状态.
此外,我们可以打表一个ones数组表示每一个状态中1的数量,以及一个mapp数组表示每一个状态中最低位的1.
所以我们需要一个lowbit函数(求得最低位的1),以及一个get函数(求出(x,y)位置可以填入的数字的状态).运用与操作,只有同时为1才可以填入.
然后本题的dfs剪枝方式在于,每一次搜索找到分支数量(即1最少的状态),然后进行搜索,这样就可以大大减少搜索次数.最后写入一个draw函数插入数据和擦除数据

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 9, M = 1 << N;
int ones[M], mapp[M];           // ones[i]表示状态i中1的个数,mapp[i]表示状态i中最低位的1
int row[N], col[N], cell[3][3]; // 行、列、九宫格的状态,用一个二进制数表示,1表示该位置可以填入数字i
string str;                  // 输入字符串
void init() // 预处理行列和九宫格的状态
{
    for (int i = 0; i < N; i++)
        row[i] = col[i] = M - 1; // 每一行,每一列都没有填数,所以状态都是M-1
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            cell[i][j] = M - 1;
}
void draw(int x, int y, int t, bool is_set) // 在(x,y)处填入数字t,is_set表示是填入还是删除
{
    if (is_set)
        str[x * N + y] = '1' + t; // 填入数字t
    else
        str[x * N + y] = '.';
    int v = 1 << t;
    if (!is_set)
        v = -v;
    row[x] -= v;             // 第t位数变为0
    col[y] -= v;             // 第t位数变为0
    cell[x / 3][y / 3] -= v; // 方块的第t位数变为0
}
int lowbit(int x) // 找最低位的1
{
    return x & -x;
}
int get(int x, int y)
{
    return row[x] & col[y] & cell[x / 3][y / 3];//求出(x,y)位置可以填入的数字的状态
}
bool dfs(int cnt)
{
    if (!cnt)
        return true;
    int minv = 10;
    int x, y;
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
            if (str[i * N + j] == '.')
            {
                int state = get(i, j);//求出(x,y)位置可以填入的数字的状态
                if (ones[state] < minv)//要找一个分支数量最少的空格
                {
                    minv = ones[state];
                    x = i, y = j;//记录下左边,开始枚举,改变搜索顺序
                }
            }
    }
    int state = get(x, y);
    for (int i = state; i; i -= lowbit(i))//枚举状态中所有的1
    {
        int t = mapp[lowbit(i)];//返回最低位的1,对应的数字
        draw(x, y, t, true);
        if (dfs(cnt - 1)) 
            return true;
        draw(x, y, t, false);
    }
    return false;//找不到方案就返回false
}
int main()
{
    // 数独问题
    for (int i = 0; i < N; i++) // 求出打表的数组,记录最低位的1
        mapp[1 << i] = i;
    for (int i = 0; i < M; i++)
        for (int j = 0; j < N; j++)
            ones[i] += i >> j & 1; // 计算每个状态中1的个数
    while (cin >> str, str[0] != 'e')
    {
        init();
        int cnt = 0;
        for (int i = 0, k = 0; i < N; i++)
            for (int j = 0; j < N; j++, k++)
                if (str[k] != '.')
                {
                    int t = str[k] - '1';
                    draw(i, j, t, true); // 填入数字t
                }
                else
                    cnt++; // 记录空格的个数
        dfs(cnt);
        cout << str << endl;
    }
    return 0;
}
```
