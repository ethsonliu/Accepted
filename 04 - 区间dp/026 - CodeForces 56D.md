[https://cn.vjudge.net/problem/CodeForces-56D](https://cn.vjudge.net/problem/CodeForces-56D)

**题意：**

一个字符串变为另一个字符串，可有三种操作：插入，置换和删除，求最少操作数，并输出操作步骤。

**分析：**

dp[i][j] 表示把 s1[1...i] 变为 s2[1...j] 的最小操作数。

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

int dp[1005][1005];
char s1[1005];
char s2[1005];

void solve(int i, int j)
{
	if (i <= 0 && j <= 0)
		return;

	if (i - 1 >= 0 && dp[i][j] == dp[i - 1][j] + 1)
	{
		solve(i - 1, j);
		printf("DELETE %d\n", j + 1);
	}
	else if (j - 1 >= 0 && dp[i][j] == dp[i][j - 1] + 1)
	{
		solve(i, j - 1);
		printf("INSERT %d %c\n", j, s2[j]);
	}
	else
	{
		solve(i - 1, j - 1);
		if (s1[i] != s2[j])
			printf("REPLACE %d %c\n", j, s2[j]);
	}
}

int main()
{
	scanf("%s", s1 + 1);
	scanf("%s", s2 + 1);

	int n1 = strlen(s1 + 1);
	int n2 = strlen(s2 + 1);

	memset(dp, 0, sizeof(dp));

	for (int i = 1; i <= n1; i++)
		dp[i][0] = i;
	for (int i = 1; i <= n2; i++)
		dp[0][i] = i;

	for (int i = 1; i <= n1; i++)
	{
		for (int j = 1; j <= n2; j++)
		{
			if (s1[i] == s2[j])
				dp[i][j] = dp[i - 1][j - 1];
			else
				dp[i][j] = min(min(dp[i - 1][j], dp[i - 1][j - 1]), dp[i][j - 1]) + 1; // 分别对应删除，替换，添加操作
		}
	}

	printf("%d\n", dp[n1][n2]);
	solve(n1, n2);

	return 0;
}
```
