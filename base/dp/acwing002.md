### [AcWing 2. 01背包问题](https://www.acwing.com/problem/content/2/)

#### 描述

有 $N$ 件物品和一个容量是 $V$ 的背包。每件物品只能使用一次。

第 $i$ 件物品的体积是 $v_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。

输出最大价值。

**输入格式**

第一行两个整数，$N，V$，用空格隔开，分别表示物品数量和背包容积。

接下来有 $N$ 行，每行两个整数 $v_i,w_i$，用空格隔开，分别表示第 $i$ 件物品的体积和价值。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

$0<N,V≤1000$

$0<v_i,w_i≤1000$

**输入样例**

```
4 5
1 2
2 4
3 4
4 5
```

**输出样例：**

```
8
```

#### 思路


    -- 状态表示
            -- 集合：从前i个物品中选，总体积不超过j的价值的集合
            -- 属性：最大值

    -- 状态计算
            当遍历到第i个物品时，存在取或者不取两种情况
            取：f[i, j] = f[i - 1, j - vi] + wi
            不取：f[i, j] = f[i - 1, j]

  
    关键的一点，为什么逆序遍历可以优化到一维？
    j - v < j 当我们优化到一维且正序遍历体积时，f[j - v] 优先被计算，后续再更新到 f[j] 时，此时的f[i][j] = max(f[i][j], f[i][j - v] + w)，与预期的公式不符，而逆序遍历体积恰好能解决这个问题，使得计算 f[j] 时，f[j - v] 仍是 f[i - 1][j - v]。


#### 代码

##### 1. 朴素算法

```c++
#include <iostream>

using namespace std;

const int N = 1010;

int n, m;
int v[N], w[N];
int f[N][N];

int main()
{
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++ ) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= n; i ++ )
    {
        for (int j = 0; j <= m; j ++ )
        {
            f[i][j] = max(f[i][j], f[i - 1][j]);
            if (j >= v[i])
                f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]);
        }
    }
    
    cout << f[n][m] << endl;
}
```

##### 2. 优化成一维

```c++
#include <iostream>

using namespace std;

const int N = 1010;

int n, m;
int v[N], w[N];
int f[N];

int main()
{
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++ ) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= n; i ++ )
        for (int j = m; j >= v[i]; j -- )
            f[j] = max(f[j], f[j - v[i]] + w[i]);
    
    cout << f[m] << endl;
}
```