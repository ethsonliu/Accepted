[https://cn.vjudge.net/problem/CodeForces-509F](https://cn.vjudge.net/problem/CodeForces-509F)

**题意：**

n 个数字组成一棵树，现给定它的 dfs 序列，求这种树有多少种。

**分析：**

dp[i][j] 表示以 i 为根到 j 的树的个数。

注意方程 dp[i + 1][k] * dp[k][j] 里，为什么是  dp[k][j]，而不是 dp[k + 1][j] 呢，因为是以 k 为根的子树，这里你可以把区间 [k + 1, j] 看作是 k 为根的子树就好理解了。

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
int b[505];
ll dp[505][505];

int main()
{
	while (~scanf("%d", &n))
	{
		for (int i = 1; i <= n; i++)
			scanf("%d", b + i);

		memset(dp, 0, sizeof(dp));

		for (int i = 1; i <= n; i++)
			dp[i][i] = 1;

		for (int g = 1; g < n; g++)
		{
			for (int i = 1; i + g <= n; i++)
			{
				int j = i + g;

				for (int k = i + 1; k <= j; k++)
				{
					if (k == j || b[i + 1] < b[k + 1])
						dp[i][j] = (dp[i][j] + dp[i + 1][k] * dp[k][j] % MOD) % MOD;
				}
			}
		}

		printf("%lld\n", dp[1][n]);
	}
	return 0;
}
```
