### [AcWing 3. 完全背包问题](https://www.acwing.com/problem/content/3/)

#### 描述

有 $N$ 种物品和一个容量是 $V$ 的背包，每种物品都有无限件可用。

第 $i$ 种物品的体积是 $v_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。

输出最大价值。

**输入格式**

第一行两个整数，$N，V$，用空格隔开，分别表示物品种数和背包容积。

接下来有 $N$ 行，每行两个整数 $v_i,w_i$，用空格隔开，分别表示第 $i$ 种物品的体积和价值。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

$0<N,V≤1000$

$0<vi,wi≤1000$

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
10
```


#### 思路

    -- 状态表示
            -- 集合：从前i个物品中选，总体积不超过j的价值的集合
            -- 属性：最大值

    -- 状态计算
	    当遍历到第i个物品时，存在取0、1、2、...、k多种选法(k为在体积不超过j时的最大值，物品无限多个)
		0: f[i][j] = f[i - 1][j]
		1: f[i][j] = f[i - 1][j - v] + w
		2: f[i][j] = f[i - 1][j - 2v] + 2w
		...
		k: f[i][j] = f[i - 1][j - kv] + kw

这时我们不难得出(**公式最后一项很重要**)：

$f[i][j] = max(f[i - 1][j], f[i - 1][j - v] + w, ... , f[i - 1][j - kv] + kw)$

$f[i][j - v] = max(f[i - 1][j - v], f[i - 1][j - 2v] + w, ... , f[i - 1][j - (k + 1)v] + kw)$  其中 $k * v$ 已经是不超过 $j$ 情况下最大的数值了，所以 $f[i - 1][j - (k + 1)v] + kw$ 这一项不应存在，应为：

$f[i][j - v] = max(f[i - 1][j - v], f[i - 1][j - 2v] + w, ... , f[i - 1][j - kv] + (k - 1)w)$

--> $f[i][j] = max(f[i - 1][j], f[i][j - v] + w)$

之后可以将数组优化至一维，与01背包优化思路类似，此时正序遍历体积满足公式。

#### 代码


##### 1. 朴素算法
```c++
#include <iostream>

using namespace std;

const int N = 1010;

int n, m;
int v, w;
int f[N][N];

int main()
{
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++ )
    {
        cin >> v >> w;
        for (int j = 0; j <= m; j ++ )
        {
            f[i][j] = max(f[i][j], f[i - 1][j]);
            if (j >= v)
                f[i][j] = max(f[i][j], f[i][j - v] + w);
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

const int N = 1010;

int n, m;
int v, w;
int f[N];

int main()
{
    cin >> n >> m;
    
    for (int i = 1; i <= n; i ++ )
    {
        cin >> v >> w;
        for (int j = v; j <= m; j ++ )
            f[j] = max(f[j], f[j - v] + w);
    }
    
    cout << f[m] << endl;
    
    return 0;
}
```