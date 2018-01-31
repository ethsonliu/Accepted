[https://cn.vjudge.net/problem/CodeForces-453B](https://cn.vjudge.net/problem/CodeForces-453B)

**题意：**

给定一个序列 a， 求一序列 b，要求 ∑|ai−bi| 最小，并且 b 中任意两数的最大公约数为 1。

**分析：**

因为 b 中不可能含有相同的因子，所以每个素数只能使用 1 次。又因为说 ai 最大为 30，所以素数只需要考虑到 57 即可。因为即使对于 30 而言，
59 和 1 的代价是一样的。

所以有 dp[i][j] 表示的是到第 i 个数，使用过的素数状态 j。

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
#include <map>
typedef long long int ll;
const int MOD = (int)1e9 + 7;
const int INF = 1000000007;
const ll INFLL = 0x3f3f3f3f3f3f3f3f;
using namespace std;

int n;
int a[105];
int dp[105][1 << 17];
int pre[105][1 << 17];
int num[105][1 << 17];
int s[60];
int prime[17] = { 2,3,5,7,11,13,17,19,23,29,31,37,39,41,43,47,53 };

void print(int i, int s)
{
	if (i == 1)
	{
		printf("%d", num[i][s]);
		return;
	}

	print(i - 1, pre[i][s]);
	printf(" %d", num[i][s]);
}

int main()
{
	while (~scanf("%d", &n))
	{
		for (int i = 1; i <= n; i++)
			scanf("%d", a + i);

		int top = 1 << 17;
		for (int i = 0; i <= n; i++)
			for (int j = 0; j < top; j++)
				dp[i][j] = INF;
		memset(s, 0, sizeof(s));
		dp[0][0] = 0;

		for (int i = 2; i < 60; i++)
		{
			for (int j = 0; j < 17; j++)
			{
				if (i % prime[j] == 0)
					s[i] |= (1 << j);
			}
		}

		for (int i = 0; i < n; i++)
		{
			for (int j = 0; j < top; j++)
			{
				if (dp[i][j] == INF)
					continue;
				for (int k = 1; k < 59; k++)
				{
					if (s[k] & j)
						continue;
					if (dp[i + 1][j | s[k]] > dp[i][j] + abs(a[i + 1] - k))
					{
						dp[i + 1][j | s[k]] = dp[i][j] + abs(a[i + 1] - k);
						pre[i + 1][j | s[k]] = j;
						num[i + 1][j | s[k]] = k;
					}
				}
			}
		}

		int ans = INF;
		int st = -1;
		for (int i = 0; i < top; i++)
		{
			if (ans > dp[n][i])
			{
				ans = dp[n][i];
				st = i;
			}
		}

		print(n, st);
		printf("\n");
	}
	return 0;
}
```
