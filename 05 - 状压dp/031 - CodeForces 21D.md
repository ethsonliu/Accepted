[https://cn.vjudge.net/problem/CodeForces-21D](https://cn.vjudge.net/problem/CodeForces-21D)

**题意：**

n 点 m 边，从点 0 出发至少经过每条边一次，所形成的欧拉回路的最小路径和。

**分析：**

首先把所有边的路径加起来，接下来看看是否存在多个连通分量，若 2 个或 2 个以上，直接输出 -1。

若只有一个，那就让它成为欧拉回路，只要让所有点的度数都为偶数即可。因此问题转化为怎样“加边”使得所形成的欧拉回路的路径和最小。

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

int n, m;
int dis[15][15];
int dp[1 << 15];
int du[15];
int ans;

int solve()
{
	int i, j, s;
	int top = 1 << n;

	for (i = 1; i < n; i++) // 如果 i 这个点有边连接，但点 0 又走不到这个点，说明和这个点相连的边是走不到的，即有多个连通分量
		if (du[i] && dis[0][i] == INF)
			return -1;

	for (i = 1; i < top; i++)
		dp[i] = INF;
	dp[0] = 0;

	for (s = 0; s < top; s++)
	{
		for (i = 0; i < n; i++)
			if ((du[i] & 1) && (s & (1 << i)))
				break;
		if (i == n)
			dp[s] = 0;

		for (i = 0; i < n; i++)
		{
			if ((du[i] & 1) && !(s & (1 << i)))
			{
				for (j = i + 1; j < n; j++)
				{
					if ((du[j] & 1) && !(s & (1 << j)) && dis[i][j] < INF)
						dp[s | (1 << i) | (1 << j)] = min(dp[s | (1 << i) | (1 << j)], dp[s] + dis[i][j]);
				}
			}
		}
	}

	if (dp[top - 1] >= INF)
		return -1;

	return dp[top - 1] + ans;
}

int main()
{
	while (~scanf("%d%d", &n, &m))
	{
		for (int i = 0; i < n; i++)
			for (int j = 0; j < n; j++)
				dis[i][j] = INF;
		memset(du, 0, sizeof(du));
		ans = 0;

		int u, v, w;
		while (m--)
		{
			scanf("%d%d%d", &u, &v, &w);
			u--, v--;
			dis[u][v] = dis[v][u] = min(dis[u][v], w);
			du[u]++, du[v]++;
			ans += w;
		}

		for (int k = 0; k < n; k++)
			for (int i = 0; i < n; i++)
				for (int j = 0; j < n; j++)
					dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
		for (int i = 0; i < n; i++)
			dis[i][i] = 0;

		printf("%d\n", solve());
	}
	return 0;
}
```
