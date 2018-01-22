[https://cn.vjudge.net/problem/CodeForces-152E](https://cn.vjudge.net/problem/CodeForces-152E)

**题意：**

n * m 矩阵格子，每个格子里有个数值，代表代价，给定 k 个格子，要求把这 k 个格子用水泥覆盖，且这 k 个格子连通，相邻两个被水泥填的格子可以相互连通，求最小的代价和及其方案。


**分析：**

很好的题目，看代码即可。

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
const int INF = 99999999;
using namespace std;

struct Node
{
	int x, y;
	int cost;
	int s;

	Node()
	{}
	Node(int x_, int y_, int cost_, int s_) : x(x_), y(y_), cost(cost_), s(s_)
	{}
};

int n, m, k;
int top;
int dp[105][105][1 << 7];
int vis[105][105][1 << 7];
int cost[105][105];
int st[105][105];
int used[105][105];
int dir[4][2] = { 0,1,0,-1,1,0,-1,0 };
Node pre[105][105][1 << 7];
queue<Node> Q;

void spfa()
{
	while (!Q.empty())
	{
		Node n1 = Q.front();
		Q.pop();
		vis[n1.x][n1.y][n1.s] = 0;
		if (n1.s == top - 1)
			continue;

		for (int i = 0; i < 4; i++)
		{
			int xx = n1.x + dir[i][0];
			int yy = n1.y + dir[i][1];
			if (xx >= 0 && xx < n && yy >= 0 && yy < m)
			{
				int ss = n1.s | st[xx][yy];
				int cc = dp[n1.x][n1.y][n1.s] + cost[xx][yy];
				if (cc < dp[xx][yy][ss])
				{
					dp[xx][yy][ss] = cc;
					pre[xx][yy][ss] = n1;
					if (ss == n1.s && !vis[xx][yy][ss])
					{
						vis[xx][yy][ss] = 1;
						Q.push(Node(xx, yy, cc, ss));
					}
				}
			}
		}
	}
}

void set_used(Node t)
{
	used[t.x][t.y] = 1;
	if (t.s == -1)
		return;
	Node tt = pre[t.x][t.y][t.s];
	set_used(tt);
	if (tt.x == t.x && tt.y == t.y)
	{
		tt.s = (t.s - tt.s) | st[t.x][t.y];
		set_used(tt);
	}
}

int main()
{
	while (~scanf("%d%d%d", &n, &m, &k))
	{
		for (int i = 0; i < n; i++)
			for (int j = 0; j < m; j++)
				scanf("%d", &cost[i][j]);

		memset(pre, -1, sizeof(pre));
		memset(st, 0, sizeof(st));
		memset(used, 0, sizeof(used));
		memset(vis, 0, sizeof(vis));
		for (int i = 0; i < n; i++)
			for (int j = 0; j < m; j++)
				for (int s = 0; s < (1 << k); s++)
					dp[i][j][s] = INF;

		int sx, sy;
		int x, y;
		for (int i = 0; i < k; i++)
		{
			scanf("%d%d", &x, &y);
			x--, y--;
			if (i == 0)
				sx = x, sy = y;
			dp[x][y][1 << i] = cost[x][y];
			st[x][y] = 1 << i;
		}

		top = 1 << k;
		for (int i = 1; i < top; i++)
		{
			for (int x = 0; x < n; x++)
			{
				for (int y = 0; y < m; y++)
				{
					if (st[x][y] && !(i & st[x][y]))
						continue;

					for (int sub = i & (i - 1); sub; sub = (sub - 1) & i)
					{
						if (dp[x][y][i] > dp[x][y][sub | st[x][y]] + dp[x][y][(i - sub) | st[x][y]] - cost[x][y])
						{
							dp[x][y][i] = dp[x][y][sub | st[x][y]] + dp[x][y][(i - sub) | st[x][y]] - cost[x][y];
							pre[x][y][i] = Node(x, y, 0, sub | st[x][y]); // 第三个参数任意值，注意 pre[][][] 的用处
						}
					}

					if (dp[x][y][i] < INF)
					{
						vis[x][y][i] = 1;
						Q.push(Node(x, y, dp[x][y][i], i));
					}
				}
			}

			spfa();
		}

		printf("%d\n", dp[sx][sy][top - 1]);

		set_used(Node(sx, sy, 0, top - 1));

		for (int i = 0; i < n; i++)
		{
			for (int j = 0; j < m; j++)
			{
				if (used[i][j])
					printf("X");
				else
					printf(".");
			}
			printf("\n");
		}
	}
	return 0;
}
```
