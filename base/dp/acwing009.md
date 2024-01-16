### [AcWing 9. 分组背包问题](https://www.acwing.com/problem/content/9/)

#### 描述

有 $N$ 组物品和一个容量是 $V$ 的背包。

每组物品有若干个，同一组内的物品最多只能选一个。

每件物品的体积是 $v_{ij}$，价值是 $w_{ij}$，其中 $i$ 是组号，$j$ 是组内编号。

求解将哪些物品装入背包，可使物品总体积不超过背包容量，且总价值最大。

输出最大价值。

**输入格式**

第一行有两个整数 $N，V$，用空格隔开，分别表示物品组数和背包容量。

接下来有 $N$ 组数据：

每组数据第一行有一个整数 $S_i$，表示第 $i$ 个物品组的物品数量；
每组数据接下来有 $S_i$ 行，每行有两个整数 $v_{ij},w_{ij}$，用空格隔开，分别表示第 $i$ 个物品组的第 $j$ 个物品的体积和价值；

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

$0<N,V≤100$

$0<S_i≤100$

$0<v_{ij},w_{ij}≤100$

**输入样例**

```
3 5
2
1 2
2 4
1
3 4
1
4 5
```

**输出样例：**

```
8
```

#### 思路

    -- 状态表示
            -- 集合：从前i组物品中选，总体积不超过j的价值的集合
            -- 属性：最大值

    -- 状态计算
	    当遍历到第i组物品时，存在取0、1、2、...、s个物品的情况
		0: f[i][j] = f[i - 1][j]
		1: f[i][j] = f[i - 1][j - v] + w
		2: f[i][j] = f[i - 1][j - 2v] + 2w
		...
		s: f[i][j] = f[i - 1][j - sv] + sw

即：

$f[i][j] = max(f[i - 1][j], f[i - 1][j - v] + w, ... , f[i - 1][j - sv] + sw)$


#### 代码


##### 1. 朴素算法

```c++
#include <iostream>

using namespace std;

const int N = 110;

int n, m;
int s[N], w[N][N], v[N][N];
int f[N][N];

int main()
{
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++ )
    {
        cin >> s[i];
        for (int j = 1; j <= s[i]; j ++ )
            cin >> v[i][j] >> w[i][j];
    }
    
    for (int i = 1; i <= n; i ++ )
    {
        for (int j = 0; j <= m; j ++ )
        {
            f[i][j] = max(f[i][j], f[i - 1][j]);
            for (int k = 1; k <= s[i]; k ++ )
            {
                if (j >= v[i][k])
                    f[i][j] = max(f[i][j], f[i - 1][j - v[i][k]] + w[i][k]);
            }
        }
    }
    
    cout << f[n][m] << endl;
    
    return 0;
}
```

##### 2. 优化至一维

```c++
#include <iostream>

using namespace std;

const int N = 110;

int n, m;
int s[N], w[N][N], v[N][N];
int f[N];

int main()
{
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++ )
    {
        cin >> s[i];
        for (int j = 1; j <= s[i]; j ++ )
            cin >> v[i][j] >> w[i][j];
    }
    
    for (int i = 1; i <= n; i ++ )
    {
        for (int j = m; j >= 0; j -- )
        {
            for (int k = 1; k <= s[i]; k ++ )
            {
                if (j >= v[i][k])
                    f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);
            }
        }
    }
    
    cout << f[m] << endl;
    
    return 0;
}
```