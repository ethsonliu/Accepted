[https://cn.vjudge.net/problem/SPOJ-ORDERS](https://cn.vjudge.net/problem/SPOJ-ORDERS)

**题意：**

有 n 个点，标号 1 到 n，给定数组 b[i] 表示 i 前面比 a[i] 小的点的个数,求 a[] 数组。

**分析：**

这题和 [POJ 2182](https://github.com/Hapoa/Accepted/blob/master/24%20-%20%E6%A0%91%E7%8A%B6%E6%95%B0%E7%BB%84/004%20-%20POJ%202182.md) 一样的思路。

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

#define MAXN 200000 + 10

int step[MAXN];
int ans[MAXN];
int c[MAXN];
int n;

int lowbit(int x)
{
	return x & (-x);
}

void update(int x, int value)
{
	while (x <= n)
	{
		c[x] += value;
		x += lowbit(x);
	}
}

int sum(int x)
{
	int s = 0;
	while (x > 0)
	{
		s += c[x];
		x -= lowbit(x);
	}
	return s;
}

int main(void)
{
	int T;
	scanf("%d", &T);

	while (T--)
	{
		memset(c, 0, sizeof(c));

		scanf("%d", &n);
		int i;
		for (i = 1; i <= n; i++)
		{
			scanf("%d", &step[i]);
			update(i, 1);
		}

		for (i = n; i >= 1; i--)
		{
			int goal = i - step[i];
			int low = 1;
			int high = n;
			while (high >= low)
			{
				int mid = (high + low) / 2;
				int s = sum(mid);
				if (s >= goal)
				{
					high = mid - 1;
				}
				else if (s < goal)
					low = mid + 1;
			}
			update(low, -1);
			ans[i] = low;
		}

		for (i = 1; i <= n; i++)
		{
			if (i == n)
				printf("%d\n", ans[i]);
			else
				printf("%d ", ans[i]);
		}
	}
	return 0;
}
```
