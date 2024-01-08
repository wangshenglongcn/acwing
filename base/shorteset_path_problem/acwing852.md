### [AcWing 852. spfa判断负环](https://www.acwing.com/problem/content/854/)

#### 描述

给定一个 $n$ 个点 $m$ 条边的有向图，图中可能存在重边和自环， 边权可能为负数。

请你判断图中是否存在负权回路。

**输入格式**

第一行包含整数 $n$ 和 $m$。

接下来 $m$ 行每行包含三个整数 $x,y,z$，表示存在一条从点 $x$ 到点 $y$ 的有向边，边长为 $z$。

**输出格式**

如果图中存在负权回路，则输出 `Yes`，否则输出 `No`。

**数据范围**


$1≤n≤2000$，

$1≤m≤10000$，

图中涉及边长绝对值均不超过 $10000$。

**输入样例：**

```
3 3
1 2 -1
2 3 4
3 1 -4
```

**输出样例：**

```
Yes
```

#### 思路

使用spfa判断负环要
1. 将全部点入队，以免有自环且负环的情况
2. `cnt[i]` 统计 `i` 点的边数，$>=n$ 时由抽屉原理，必然存在负环（正环无意义）。

#### 代码

```c++
#include <iostream>
#include <cstring>
#include <queue>

using namespace std;

const int N = 100010;

int n, m;
int h[N], e[N], w[N], ne[N], idx;
int d[N], cnt[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

bool spfa()
{
    memset(d, 0x3f, sizeof d);
    
    queue<int> q;
    for (int i = 1; i <= n; i ++ )
    {
        q.push(i);
        st[i] = true;
    }
    
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
                cnt[j] = cnt[t] + 1;
                if (cnt[j] >= n) return true;
                
                if (!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    
    return false;
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
    
    if (spfa()) puts("Yes");
    else puts("No");
    
    return 0;
}
```