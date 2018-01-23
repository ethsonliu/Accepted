[https://cn.vjudge.net/problem/CodeForces-417D](https://cn.vjudge.net/problem/CodeForces-417D)

**题意：**

n 个朋友解决 m 道题目，每个监视器 b 花费，给出 n 个朋友的佣金，所需要的监视器数，以及可以完成的题目序号。
注意，这里只要你拥有的监视器数量大于小伙伴需要的监视器数量即可。求最少花费多少金额可以解决所有问题。

**分析：**

直接看代码即可。

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
const int INF1 = 99999999;
const ll INF2 = 0x3f3f3f3f3f3f3f3f;
using namespace std;

struct State
{
	int s;
	ll k, val;

	bool operator<(State s1)
	{
		return k < s1.k;
	}
};

int n, m, b;
int top;
ll dp[1 << 20];
State p[105];

int main()
{
	while (~scanf("%d%d%d", &n, &m, &b))
	{
		for (int i = 0; i < (1 << m); i++)
			dp[i] = INF2;

		int t, a;
		for (int i = 0; i < n; i++)
		{
			p[i].s = 0;
			scanf("%lld %lld %d", &p[i].val, &p[i].k, &t);
			for (int j = 0; j < t; j++)
			{
				scanf("%d", &a);
				p[i].s |= (1 << (a - 1));
			}
		}

		sort(p, p + n);

		dp[0] = 0;
		top = 1 << m;
		ll ans = INF2;

		for (int i = 0; i < n; i++)
		{
			for (int j = 0; j < top; j++)
			{
				if (dp[j] == INF2)
					continue;

				dp[j | p[i].s] = min(dp[j | p[i].s], dp[j] + p[i].val);
			}

			ans = min(ans, dp[top - 1] + p[i].k * b);
		}

		if (ans == INF2)
			printf("-1\n");
		else
			printf("%lld\n", ans);
	}
	return 0;
}
```
