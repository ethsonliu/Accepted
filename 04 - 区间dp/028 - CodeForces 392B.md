[https://cn.vjudge.net/problem/CodeForces-392B](https://cn.vjudge.net/problem/CodeForces-392B)

**题意：**

给定一个 3 * 3 的矩阵 [i, j] 表示把一个盘子从第 i 根柱子移到第 j 根柱子的花费，下面一行 n 表示一共有 n 个盘子在 1 号柱子，
问全部都移动到 3 号柱子的最小花费。

**分析：**

dp[n][i][j] 表示n 个盘子从 i 移动到 j 上的最少花费。

1. 将 1 上的 n-1 个盘子移动到 2 上,将 1 上的最后一个移动到 3 上,再将 2 上的 n-1 个盘子移动到 3上。

2. 将 1 上的 n-1 个盘子移动到 3 上,将 1 上的最后一个移动到 2 上,将 3 上的 n-1 个盘子移动回 1 上,将 2 上的最后一个盘子移动到 3 上,最后将 1 上的 n-1 个盘子
移动到 3 上。

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <cmath>
#include <queue>
#include <stack>
#include <vector>
typedef long long int ll;
const ll MOD = 1e9 + 7;
const ll INF = (1 << 64) - 1;
using namespace std;

int n;
int a[5][5];
ll dp[45][5][5];

ll solve(int n, int i, int j)
{
	if (dp[n][i][j] != INF)
		return dp[n][i][j];

	int t = 6 - i - j; // 因为 1 + 2 + 3  = 6，其中 1,2,3 分别是第一根，第二根，第三根柱子

	dp[n][i][j] = solve(n - 1, i, t) + a[i][j] + solve(n - 1, t, j);
	dp[n][i][j] = min(dp[n][i][j], solve(n - 1, i, j) * 2 + a[i][t] + solve(n - 1, j, i) + a[t][j]);

	return dp[n][i][j];
}

int main()
{
	for (int i = 1; i <= 3; i++)
		for (int j = 1; j <= 3; j++)
			scanf("%d", &a[i][j]);

	scanf("%d", &n);

	for (int i = 1; i <= 40; i++)
		for (int j = 1; j <= 3; j++)
			for (int k = 1; k <= 3; k++)
				dp[i][j][k] = INF;

	for (int i = 1; i <= 3; i++)
		for (int j = 1; j <= 3; j++)
			dp[0][i][j] = 0;

	printf("%lld\n", solve(n, 1, 3));

	return 0;
}
```
