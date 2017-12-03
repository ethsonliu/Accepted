[https://cn.vjudge.net/problem/CodeForces-437E](https://cn.vjudge.net/problem/CodeForces-437E)

**题意：**

给出一个多边形，问说有多少种分割方法，将多边形分割为多个三角形。

**分析：**

dp[i][j] 表示顶点 i 到 j 的分割方法总数。

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

struct Node
{
	ll x, y; // 必须是 ll 类型的，int 的太小

	Node operator-(Node node) const
	{
		Node t;
		t.x = x - node.x;
		t.y = y - node.y;
		return t;
	}

	ll operator*(Node node) const
	{
		return x * node.y - y * node.x;
	}
};

int n;
Node node[205];
ll dp[205][205];

ll dfs(int l, int r)
{
	if (dp[l][r] != -1)
		return dp[l][r];

	if (l + 1 == r)
	{
		dp[l][r] = 1;
		return dp[l][r];
	}

	dp[l][r] = 0;

	for (int k = l + 1; k < r; k++)
	{
		if ((node[l] - node[r]) * (node[k] - node[r]) < 0)
			dp[l][r] = (dp[l][r] + dfs(l, k) * dfs(k, r) % MOD) % MOD;
	}

	return dp[l][r];
}

int main()
{
	while (~scanf("%d", &n))
	{
		for (int i = 0; i < n; i++)
			scanf("%lld %lld", &node[i].x, &node[i].y);

		// 保证顺时针
		ll tmp = 0;
		node[n].x = node[0].x, node[n].y = node[0].y;
		for (int i = 0; i < n; i++)
			tmp += (node[i] * node[i + 1]);
		if (tmp > 0)
			reverse(node, node + n);

		memset(dp, -1, sizeof(dp));

		printf("%lld\n", dfs(0, n - 1));
	}
	return 0;
}
```
