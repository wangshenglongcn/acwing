### [AcWing 853. 有边数限制的最短路](https://www.acwing.com/problem/content/855/)

#### 描述

给定一个 $n$ 个点 $m$ 条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你求出从 $1$ 号点到 $n$ 号点的最多经过 $k$ 条边的最短距离，如果无法从 $1$ 号点走到 $n$ 号点，输出 `impossible`。

注意：图中可能 **存在负权回路** 。

**输入格式**

第一行包含三个整数 $n,m,k$。

接下来 $m$ 行，每行包含三个整数 $x,y,z$，表示存在一条从点 $x$ 到点 $y$ 的有向边，边长为 $z$。

点的编号为 $1∼n$。

**输出格式**

输出一个整数，表示从 $1$ 号点到 $n$ 号点的最多经过 $k$ 条边的最短距离。

如果不存在满足条件的路径，则输出 `impossible`。

**数据范围**

$1≤n,k≤500$，

$1≤m≤10000$，

$1≤x,y≤n$，

任意边长的绝对值不超过 $10000$。

**输入样例：**

```
3 3 1
1 2 1
2 3 1
1 3 3
```

**输出样例：**

```
3
```

#### 思路

定义 `d[N]、last[N]`，其中 `last[N]` 用来存储每一次 `d[N]` 的状态
1. 遍历k次，k即为边数限制
2. 每次遍历所有边，进行一次松弛操作，即 `d[b] = min(d[b], last[a] + w)`

#### 代码

```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 510, M = 10010;

struct Edge
{
    int a, b, c;
}edges[M];

int n, m, k;
int d[N], last[N];

int bellman_ford()
{
    memset(d, 0x3f, sizeof d);
    
    d[1] = 0;
    for (int i = 0; i < k; i ++ )
    {
        memcpy(last, d, sizeof d);
        for (int j = 0; j < m; j ++ )
        {
            auto e = edges[j];
            d[e.b] = min(d[e.b], last[e.a] + e.c);
        }
    }
    
    return d[n];
}

int main()
{
    scanf("%d%d%d", &n, &m, &k);
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        edges[i] = {a, b, c};
    }
    
    int t = bellman_ford();
    if (t > 0x3f3f3f3f / 2) puts("impossible");
    else printf("%d\n", t);
    
    return 0;
}
```