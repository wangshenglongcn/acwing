### [AcWing 859. Kruskal算法求最小生成树](https://www.acwing.com/problem/content/861/)

#### 描述

给定一个 $n$ 个点 $m$ 条边的无向图，图中可能存在重边和自环，边权可能为负数。

求最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

给定一张边带权的无向图 $G=(V,E)$，其中 $V$ 表示图中点的集合，$E$ 表示图中边的集合，$n=|V|$，$m=|E|$。

由 $V$ 中的全部 $n$ 个顶点和 $E$ 中 $n−1$ 条边构成的无向连通子图被称为 $G$ 的一棵生成树，其中边的权值之和最小的生成树被称为无向图 $G$ 的最小生成树。

**输入格式**

第一行包含两个整数 $n$ 和 $m$。

接下来 $m$ 行，每行包含三个整数 $u,v,w$，表示点 $u$ 和点 $v$ 之间存在一条权值为 $w$ 的边。

**输出格式**

共一行，若存在最小生成树，则输出一个整数，表示最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

**数据范围**

$1≤n≤10^5$,

$1≤m≤2∗10^5$,

图中涉及边的边权的绝对值均不超过 $1000$。

**输入样例：**

```
4 5
1 2 1
1 3 2
1 4 3
2 3 2
3 4 4
```

**输出样例：**

```
6
```

#### 思路

思路如下
定义 `res` 存储结果，定义 `cnt` 存储经历的边数，边数 $=n-1$ 时输出 `res`
1. 边按权值排序
2. 遍历所有边，假如边两边不在一个并查集中，则结果加上改边权值

#### 代码

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010, M = 200010;

struct Edge
{
    int a, b, c;
}edges[M];

int n, m;
int p[N];

bool opt(struct Edge A, struct Edge B)
{
    return A.c < B.c;
}

int find(int x)
{
    if (x != p[x]) p[x] = find(p[x]);
    
    return p[x];
}

int main()
{
    scanf("%d%d", &n, &m);
    
    for (int i = 1; i <= n; i ++ ) p[i] = i;
    
    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        edges[i] = {a, b, c};
    }
    
    sort(edges, edges + m, opt);
    
    int res = 0, cnt = 0;
    for (int i = 0; i < m; i ++ )
    {
        auto e = edges[i];
        int a = find(e.a), b = find(e.b), w = e.c;
        if (a != b)
        {
            res += w;
            cnt ++ ;
            p[a] = b;
        }
    }
    
    if (cnt == n - 1) printf("%d\n", res);
    else puts("impossible");
    
    return 0;
}
```