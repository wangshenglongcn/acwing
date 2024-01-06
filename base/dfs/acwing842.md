### [AcWing 842. 排列数字](https://www.acwing.com/problem/content/844/)

#### 描述


#### 思路


#### 代码

```c++
#include <iostream>

using namespace std;

const int N = 15;

int n;
int path[N];
bool st[N];

void dfs(int u)
{
    if (u == n)
    {
        for (int i = 0; i < n; i ++ ) cout << path[i] << " ";
        cout << endl;
    }
    
    for (int i = 1; i <= n; i ++ )
    {
        if (!st[i])
        {
            path[u] = i;
            st[i] = true;
            dfs(u + 1);
            st[i] = false;
        }
    }
}

int main()
{
    cin >> n;
    
    dfs(0);
    
    return 0;
}
```