[https://cn.vjudge.net/problem/CodeForces-31E](https://cn.vjudge.net/problem/CodeForces-31E)

**题意：**

2 * n 长的数字，从左端开始，分配给 S1，S2 两个空串的高位，要求最后 S1，S2 两个串的长度相同（允许前导 0），且 S1，S2 两串转换为数字后，和最大。输出原串每一位的分配方案。

**分析：**

dp[i][j] 表示数字前 i 位中选 j 个放进 S1 后的和的最大值。

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

int n;
char s[40];
ll dp[40][20];
int c[40][20];
int p[40];

int main()
{
	while(~scanf("%d",&n))
	{
		scanf("%s", s);

		memset(dp, -1, sizeof(dp));
		memset(c, 0, sizeof(c));
		memset(p, 0, sizeof(p));

		dp[1][0] = dp[1][1] = (s[0] - '0') * (ll)pow(10.0, n - 1);
		c[1][0] = 0; // 0 表示放进 S2
		c[1][1] = 1; // 1 表示放进 S1

		for (int i = 1; i < 2 * n; i++)
		{
			int j_top = min(i, n);

			for (int j = 0; j <= j_top; j++)
			{
				if ((i + 1) - j <= n) // 放进 S2
				{
					ll t = dp[i][j] + (s[i] - '0') * (ll)pow(10.0, n - i + j - 1);
					if (t > dp[i + 1][j])
					{
						dp[i + 1][j] = t;
						c[i + 1][j] = 0;
					}
				}
				if (j < n) // 放进 S1
				{
					ll t = dp[i][j] + (s[i] - '0') * (ll)pow(10.0, n - j - 1);
					if (t > dp[i + 1][j + 1])
					{
						dp[i + 1][j + 1] = t;
						c[i + 1][j + 1] = 1;
					}
				}
			}
		}

		int n1 = 2 * n;
		int n2 = n;
		while (n1)
		{
			if (c[n1][n2])
			{
				p[n1] = 1;
				n2--;
			}
			else
				p[n1] = 0;

			n1--;
		}

		for (int i = 1; i <= 2 * n; i++)
		{
			if (p[i])
				printf("H");
			else
				printf("M");
		}
		printf("\n");
	}
	return 0;
}
```
