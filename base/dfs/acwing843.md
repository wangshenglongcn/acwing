### [AcWing 843. n-皇后问题](https://www.acwing.com/problem/content/845/)

#### 描述


#### 思路



#### 代码

```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 15, M = 30;

int n;
char path[N][N];
bool col[N], dg[M], udg[M];

void dfs(int u)
{
    if (u == n)
    {
        for (int i = 0; i < n; i ++ ) puts(path[i]);
        puts("");
    }
    
    for (int i = 0; i < n; i ++ )
    {
        if (!col[i] && !udg[u + i] && !dg[u - i + n])
        {
            col[i] = udg[u + i] = dg[u - i + n] = true;
            path[u][i] = 'Q';
            dfs(u + 1);
            path[u][i] = '.';
            col[i] = udg[u + i] = dg[u - i + n] = false;
        }
    }
}

int main()
{
    cin >> n;
    
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            path[i][j] = '.';
        
    dfs(0);
    
    return 0;
}
```