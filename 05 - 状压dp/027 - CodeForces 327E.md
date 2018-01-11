[https://cn.vjudge.net/problem/CodeForces-327E](https://cn.vjudge.net/problem/CodeForces-327E)

**题意：**

给定一个整数序列，任意排序，要求所有的前缀和不能等于给定的几个数，求排列方式。

**分析：**

dp[i] 表示 i 状态下的排列方式。

lowbit(i) 求出 i 的最低位，即去除 i 的所有高位，仅保留最低位的 1。

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
const int MOD = 1e9 + 7;
const int INF = 99999999;
using namespace std;

int n, k;
int a[24];
int bad[3];
int dp[1 << 24];
int sum[1 << 24];

inline int lowbit(int i)
{
	return i & (-i);
}

int main()
{
	while (~scanf("%d", &n))
	{
		memset(sum, 0, sizeof(sum));

		for (int i = 0; i < n; i++)
		{
			scanf("%d", a + i);
			sum[1 << i] = a[i];
		}

		scanf("%d", &k);

		for (int i = 0; i < k; i++)
			scanf("%d", bad + i);

		memset(dp, 0, sizeof(dp));
		dp[0] = 1;

		for (int i = 1; i < (1 << n); i++)
		{
			sum[i] = sum[i & ~lowbit(i)] + sum[lowbit(i)];

			if (sum[i] == bad[0] || sum[i] == bad[1])
				continue;

			for (int j = i; j != 0; j -= lowbit(j))
			{
				dp[i] += dp[i & ~lowbit(j)];
				dp[i] %= MOD;
			}
		}

		printf("%d\n", dp[(1 << n) - 1]);
	}
	return 0;
}
```
