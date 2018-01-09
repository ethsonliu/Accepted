[https://cn.vjudge.net/problem/CodeForces-544E](https://cn.vjudge.net/problem/CodeForces-544E)

**题意：**

n 个长度均为 m 的字符串。定义：一个字符串为 easy to remember 的前提是，存在一个字母，使得该字母在当前列是唯一的。

通过改变字母使得 n 个字符串都为 easy to remember ，再给出 n 个字符串每个位置要改变字符需要的花费。求 n 个字符串全部变为 easy to remember 的最小总花费。

**分析：**

我们要一列一列的做，先做第一列，对它们状压 dp。

dp[i] 表示已 easy to remember 的状态为 i 的最小花费。

会出现两个转移：

1. 直接这个字母；

2. 把这一列中与这个字母相同的字母全修改为各异的字母。

```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <algorithm>
#include <cstring>
#include <cmath>
#include <queue>
#include <stack>
#include <vector>
#include <bitset>
typedef long long int ll;
const int MOD = 9973;
const int INF = 99999999;
using namespace std;

int n, m;
char s[25][25];
int a[25][25];
int cost[25][25];
int bit[25][25];
int dp[1 << 20];

int main()
{
	while (~scanf("%d %d", &n, &m))
	{
		for (int i = 0; i < n; i++)
			scanf("%s", s + i);

		for (int i = 0; i < n; i++)
			for (int j = 0; j < m; j++)
				scanf("%d", &a[i][j]);

		memset(bit, 0, sizeof(bit));

		for (int i = 0; i < n; i++)
		{
			for (int j = 0; j < m; j++)
			{
				int sum = 0;
				int max_cost = -1;
				for (int k = 0; k < n; k++)
				{
					if (s[i][j] == s[k][j])
					{
						sum += a[k][j];
						max_cost = max(max_cost, a[k][j]);
						bit[i][j] |= (1 << k);
					}
				}

				sum -= max_cost;
				cost[i][j] = sum;
			}
		}

		for (int i = 0; i < (1 << n); i++)
			dp[i] = INF;
		dp[0] = 0;

		for (int k = 0; k < m; k++)
		{
			for (int i = 0; i < (1 << n); i++)
			{
				for (int j = 0; j < n; j++)
				{
					if (i & (1 << j))
						continue;

					dp[i | (1 << j)] = min(dp[i | (1 << j)], dp[i] + a[j][k]);
					dp[i | bit[j][k]] = min(dp[i | bit[j][k]], dp[i] + cost[j][k]);
				}
			}
		}

		printf("%d\n", dp[(1 << n) - 1]);
	}
	return 0;
}
```
