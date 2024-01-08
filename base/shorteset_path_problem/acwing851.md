### [AcWing 851. spfa求最短路](https://www.acwing.com/problem/content/853/)

#### 描述

给定一个 $n$ 个点 $m$ 条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你求出 $1$ 号点到 $n$ 号点的最短距离，如果无法从 $1$ 号点走到 $n$ 号点，则输出 `impossible`。

数据保证不存在负权回路。

**输入格式**

第一行包含整数 $n$ 和 $m$。

接下来 $m$ 行每行包含三个整数 $x,y,z$，表示存在一条从点 $x$ 到点 $y$ 的有向边，边长为 $z$。

**输出格式**

输出一个整数，表示 $1$ 号点到 $n$ 号点的最短距离。

如果路径不存在，则输出 `impossible`。

**数据范围**

$1≤n,m≤10^5$,

图中涉及边长绝对值均不超过 $10000$。

**输入样例：**

```
3 3
1 2 5
2 3 -3
1 3 4
```

**输出样例：**

```
2
```

#### 思路

spfa 算法是 bellman ford 算法的优化，最差情况下是 $O(nm)$

定义 `st[N]` 来标识点是否入队，`d[N]` 存储点到起点的距离。

接着通过队列来进行持续的入队出队操作，直至结束。

#### 代码

```c++
#include <iostream>
#include <cstring>
#include <queue>

using namespace std;

const int N = 100010;

int n, m;
int h[N], e[N], w[N], ne[N], idx;
int d[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int spfa()
{
    memset(d, 0x3f, sizeof d);
    
    queue<int> q;
    q.push(1);
    st[1] = true;
    d[1] = 0;
    
    while (q.size())
    {
        int t = q.front();
        q.pop();
        st[t] = false;
        
        for (int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (d[j] > d[t] + w[i])
            {
                d[j] = d[t] + w[i];
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    
    return d[n];
}

int main()
{
    scanf("%d%d", &n, &m);
    
    memset(h, -1, sizeof h);
    
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }
    
    int t = spfa();
    if (t == 0x3f3f3f3f) puts("impossible");
    else printf("%d\n", t);
    
    return 0;
}
```