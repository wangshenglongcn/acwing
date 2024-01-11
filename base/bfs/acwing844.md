### [AcWing 844. 走迷宫](https://www.acwing.com/problem/content/846/)

#### 描述

给定一个 $n×m$ 的二维整数数组，用来表示一个迷宫，数组中只包含 $0$ 或 $1$，其中 $0$ 表示可以走的路，$1$ 表示不可通过的墙壁。

最初，有一个人位于左上角 $(1,1)$ 处，已知该人每次可以向上、下、左、右任意一个方向移动一个位置。

请问，该人从左上角移动至右下角 $(n,m)$ 处，至少需要移动多少次。

数据保证 $(1,1)$ 处和 $(n,m)$ 处的数字为 $0$，且一定至少存在一条通路。

**输入格式**

第一行包含两个整数 $n$ 和 $m$。

接下来 $n$ 行，每行包含 $m$ 个整数（$0$ 或 $1$），表示完整的二维数组迷宫。

**输出格式**

输出一个整数，表示从左上角移动至右下角的最少移动次数。

**数据范围**

$1≤n,m≤100$

**输入样例：**

```
5 5
0 1 0 0 0
0 1 0 1 0
0 0 0 0 0
0 1 1 1 0
0 0 0 1 0
```

**输出样例：**

```
8
```

#### 思路

bfs由队列辅助实现，定义g[N][N]存储数组，d[N][N]存储距离，

从(1, 1)开始向四个方向延申并判断是否满足条件，直至队列为空。

#### 代码

```c++
#include <iostream>
#include <cstring>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 110;

int n, m;
int g[N][N];
int d[N][N];

int bfs()
{
    memset(d, -1, sizeof d);
    
    queue<PII> q;
    q.push({1, 1});
    d[1][1] = 0;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    while (q.size())
    {
        auto t = q.front();
        q.pop();
        
        for (int i = 0; i < 4; i ++ )
        {
            int a = t.first + dx[i], b = t.second + dy[i];
            if (a >= 1 && a <= n && b >= 1 && b <= m && g[a][b] == 0 && d[a][b] == -1)
            {
                d[a][b] = d[t.first][t.second] + 1;
                q.push({a, b});
            }
        }
    }
    
    return d[n][m];
}


int main()
{
    scanf("%d%d", &n, &m);
    
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
            cin >> g[i][j];
    
    printf("%d\n", bfs());
    
    return 0;
}
```