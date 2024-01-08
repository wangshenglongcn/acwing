### [AcWing 850. Dijkstra 求最短路 II](https://www.acwing.com/problem/content/852/)

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

$1≤n,m≤1.5×10^5,$

图中涉及边长均不小于 $0$，且不超过 $10000$。

数据保证：如果最短路存在，则最短路的长度不超过 $10^9$。

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

经查看，我们可以确定朴素的 dijkstra 算法第 2 步获取距离起点最近的点这一步可以优化，我们可以用堆来存储距离与点，在 O(1)时间内完成该步骤。

#### 代码

```c++
#include <iostream>
#include <cstring>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 1e6 + 10;

int n, m;
int h[N], e[N], w[N], ne[N], idx;
int d[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int dijkstra()
{
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    d[1] = 0;
    heap.push({0, 1});  // 按前面的元素排序，要获取距离最近的点，所以把距离放在前

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.second, dist = t.first;

        // 去除冗余节点
        if (st[ver]) continue;
        st[ver] = true;

        for (int i = h[ver]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (d[j] > dist + w[i])
            {
                d[j] = dist + w[i];
                heap.push({d[j], j});
            }
        }
    }

    return d[n];
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);
    memset(d, 0x3f3f3f3f, sizeof d);

    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }

    auto t = dijkstra();
    if (t == 0x3f3f3f3f) puts("-1");
    else printf("%d", t);

    return 0;
}
```
