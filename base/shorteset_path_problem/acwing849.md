### [AcWing 849. Dijkstra 求最短路 I](https://www.acwing.com/problem/content/851/)

#### 描述

给定一个 $n$ 个点 $m$ 条边的有向图，图中可能存在重边和自环，所有边权均为正值。

请你求出 $1$ 号点到 $n$ 号点的最短距离，如果无法从 $1$ 号点走到 $n$ 号点，则输出 $−1$。

**输入格式**

第一行包含整数 $n$ 和 $m$。

接下来 $m$ 行每行包含三个整数 $x,y,z$，表示存在一条从点 $x$ 到点 $y$ 的有向边，边长为 $z$。

**输出格式**

输出一个整数，表示 $1$ 号点到 $n$ 号点的最短距离。

如果路径不存在，则输出 $−1$。

**数据范围**

$1≤n≤500,1≤m≤10^5$，

图中涉及边长均不超过$10000$。

**输入样例：**

```
3 3
1 2 2
2 3 1
1 3 4
```

**输出样例：**

```
3
```

#### 思路

定义 d[i]存储点 i 到起点的距离，st[i]存储点 i 到起点的距离是否已经确定。

1. 遍历 n 次
2. 每次找到距离起点最近的点 t
3. 用点 t 更新其他点到起点的距离

#### 代码

```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 510;

int n, m;
int g[N][N];
int d[N];
bool st[N];

int dijkstra()
{
    d[1] = 0;

    for (int i = 0; i < n; i ++ )
    {
        int t = -1;
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || d[t] > d[j]))
                t = j;

        for (int j = 1; j <= n; j ++ )
            d[j] = min(d[j], d[t] + g[t][j]);

        st[t] = true;
    }

    return d[n];
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(g, 0x3f3f3f3f, sizeof g);
    memset(d, 0x3f3f3f3f, sizeof d);

    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = min(g[a][b], c);
    }

    int t = dijkstra();
    if (t == 0x3f3f3f3f) puts("-1");
    else printf("%d", t);

    return 0;
}
```
