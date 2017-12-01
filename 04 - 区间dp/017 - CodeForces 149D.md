[https://cn.vjudge.net/problem/CodeForces-149D](https://cn.vjudge.net/problem/CodeForces-149D)

**题意：**

给一个给定括号序列，给该括号上色，上色有三个要求

1、只有三种上色方案，不上色，上红色，上蓝色

2、每对括号必须只能给其中的一个上色

3、相邻的两个不能上同色，可以都不上色

求一共有多少种上色方案。

**分析：**

因为是一个合法的括号序列，所以每个括号与之匹配的位置是一定的，这我们首先要知道。

dp[l][r][x][y] 表示区间 [l, r] 在左端点涂 x 色，右端点涂 y 色的情况下的方案数。其中 0 代表不涂色， 1 代表涂红色， 2 代表涂蓝色。

match[] 则代表与之对应的这一对括号的位置。

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
const int INF = 99999999;
using namespace std;

int n;
char s[705];
int match[705];
ll dp[705][705][3][3];

void dfs(int l, int r)
{
	if (l + 1 == r) // 为什么都是 1 呢？看清题意，给定的括号序列合法匹配的，所以到这一步只有"()"这一种情况
	{
		dp[l][r][0][1] = 1;
		dp[l][r][1][0] = 1;
		dp[l][r][0][2] = 1;
		dp[l][r][2][0] = 1;
		return;
	}

	if (match[l] == r)
	{
		dfs(l + 1, r - 1);

		for (int i = 0; i < 3; i++)
		{
			for (int j = 0; j < 3; j++)
			{
				if (j != 1)
					dp[l][r][0][1] = (dp[l][r][0][1] + dp[l + 1][r - 1][i][j]) % MOD;
				if (i != 1)
					dp[l][r][1][0] = (dp[l][r][1][0] + dp[l + 1][r - 1][i][j]) % MOD;
				if (j != 2)
					dp[l][r][0][2] = (dp[l][r][0][2] + dp[l + 1][r - 1][i][j]) % MOD;
				if (i != 2)
					dp[l][r][2][0] = (dp[l][r][2][0] + dp[l + 1][r - 1][i][j]) % MOD;
			}
		}
	}
	else
	{
		dfs(l, match[l]);
		dfs(match[l] + 1, r);

		for (int i = 0; i < 3; i++)
			for (int j = 0; j < 3; j++)
				for (int x = 0; x < 3; x++)
					for (int y = 0; y < 3; y++)
						if (!((x == 1 && y == 1) || (x == 2 && y == 2)))
							dp[l][r][i][j] = (dp[l][r][i][j] + dp[l][match[l]][i][x] * dp[match[l] + 1][r][y][j] % MOD) % MOD;
	}
}

int main()
{
	while (~scanf("%s", s + 1))
	{
		n = strlen(s + 1);

		stack<int> S;
		for (int i = 1; i <= n; i++)
		{
			if (s[i] == '(')
				S.push(i);
			else
			{
				match[S.top()] = i;
				S.pop();
			}
		}
		memset(dp, 0, sizeof(dp));

		dfs(1, n);

		ll ans = 0;
		for (int i = 0; i < 3; i++)
			for (int j = 0; j < 3; j++)
				ans = (ans + dp[1][n][i][j]) % MOD;

		printf("%lld\n", ans);
	}
	return 0;
}
```
