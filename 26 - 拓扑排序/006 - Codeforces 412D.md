[https://cn.vjudge.net/problem/CodeForces-412D](https://cn.vjudge.net/problem/CodeForces-412D)

**题意：**

R1公司的CEO要给员工发奖金，员工一个个地进CEO的办公室领奖金。但有的员工之间有债务关系，若A君欠B君的钱，当A君领完奖金后下一个领奖金的是B君的时候，
他们就会相遇，A君就要马上还钱给B君，那么A君领到奖金的高兴心情也就变得没有那么高兴了。CEO不希望出现这种情况，
要你帮他列出一个不会发生这种情况的召唤员工的顺序~若无这种顺序则输出-1.（不会出现A欠B钱，B也欠A钱的情况）

**分析：**

若 A 欠 B 的钱，则他们的边的关系为 A->B，最后 dfs 搜索，从底往上输出结果即可。

```c++
#include <iostream>
#include <cstdio>
#include <stdio.h>
#include <cstdlib>
#include <algorithm>
#include <cstring>
#include <cmath>
#include <queue>
#include <stack>
#include <vector>
#include <bitset>
#include <utility>
#include <map>
#include <set>
#include <climits>

using namespace std;

const int N = 3 * 1e4 + 5;

int n, m, v[N], r[N];
vector<int> g[N];

void init()
{
	scanf("%d%d", &n, &m);
	int a, b;
	while (m--)
	{
		scanf("%d%d", &a, &b);
		g[a].push_back(b);
	}
	m = 0;
}

void dfs(int u)
{
	v[u] = 1;

	int t = g[u].size();
	for (int i = 0; i < t; i++)
	{
		if (v[g[u][i]])
			continue;
		dfs(g[u][i]);
	}

	r[m++] = u;
}

int main()
{
	init();

	for (int i = 1; i <= n; i++)
	{
		if (v[i]) continue;
		dfs(i);
	}

	printf("%d", r[0]);
	for (int i = 1; i < m; i++)
		printf(" %d", r[i]);
	printf("\n");

	return 0;
}
```
