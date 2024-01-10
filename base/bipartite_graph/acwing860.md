### [AcWing 860. 染色法判定二分图](https://www.acwing.com/problem/content/862/)

#### 描述

给定一个 $n$ 个点 $m$ 条边的无向图，图中可能存在重边和自环。

请你判断这个图是否是二分图。

**输入格式**

第一行包含两个整数 $n$ 和 $m$。

接下来 $m$ 行，每行包含两个整数 $u$ 和 $v$，表示点 $u$ 和点 $v$ 之间存在一条边。

**输出格式**

如果给定图是二分图，则输出 `Yes`，否则输出 `No`。

**数据范围**

$1≤n,m≤10^5$

**输入样例：**

```
4 4
1 3
1 4
2 3
2 4
```

**输出样例：**

```
Yes
```

#### 思路

可以使用dfs或bfs来实现，这里使用dfs，避免更多的代码。

#### 代码

```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 100010, M = 200010;

int n, m;
int h[N], e[M], ne[M], idx;
int color[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

bool dfs(int u, int c)
{
    color[u] = c;
    
    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        if (!color[j])
        {
            if (!dfs(j, 3 - c)) return false;
        }
        else if (color[j] == c) return false;
    }
    
    return true;
}

int main()
{
    scanf("%d%d", &n, &m);
    
    memset(h, -1, sizeof h);
    
    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        add(a, b);
        add(b, a);
    }
    
    bool flag = true;
    for (int i = 1; i <= n; i ++ )
    {
        if (!color[i])
        {
            if (!dfs(i, 1))
            {
                flag = false;
                break;
            }
        }
    }
    
    if (flag) puts("Yes");
    else puts("No");
    
    return 0;
}
```
