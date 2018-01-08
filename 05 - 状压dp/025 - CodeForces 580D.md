[https://cn.vjudge.net/problem/CodeForces-580D](https://cn.vjudge.net/problem/CodeForces-580D)

**题意：**

n 道菜，每道菜一个开心值，选出 m 道，k 条规则，x, y, z 表示 x 在 y 之前吃可以额外获得 z 的开心值，求吃完 m 道所获得的最大开心值。

**分析：**

dp[i][j] 表示已被吃的状态为 i 时，j 是最后一道被吃的最大开心值。

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

int n, m, k;
int a[20];
int c[20][20];
ll dp[1 << 18][20];

int count(int i)
{
	int nums = 0;

	while (i)
	{
		i &= (i - 1);
		nums++;
	}

	return nums;
}

int main()
{
	while (~scanf("%d %d %d", &n, &m, &k))
	{
		for (int i = 0; i < n; i++)
			scanf("%d", a + i);

		memset(c, 0, sizeof(c));

		int x, y, z;
		for (int i = 0; i < k; i++)
		{
			scanf("%d %d %d", &x, &y, &z);
			c[x - 1][y - 1] = z;
		}

		memset(dp, 0, sizeof(dp));
		for (int i = 0; i < n; i++)
			dp[1 << i][i] = a[i];

		for (int i = 0; i < (1 << n); i++)
		{
			for (int j = 0; j < n; j++)
			{
				if (i & (1 << j))
				{
					for (int k = 0; k < n; k++)
					{
						if (i & (1 << k))
							continue;

						dp[i | (1 << k)][k] = max(dp[i | (1 << k)][k], dp[i][j] + c[j][k] + a[k]);
					}
				}
			}
		}

		ll ans = 0;
		for (int i = 0; i < (1 << n); i++)
		{
			if (count(i) == m)
			{
				for (int j = 0; j < n; j++)
					ans = max(ans, dp[i][j]);
			}
		}

		printf("%lld\n", ans);
	}
	return 0;
}
```
