[https://cn.vjudge.net/problem/SPOJ-NWERC11H](https://cn.vjudge.net/problem/SPOJ-NWERC11H)

**题意：**

给定 13 张各不相同的扑克牌，问最少需要几手打出，每手打出的牌必须符合以下任意标准之一：

1、任意单张

2、相同数字 2 张

3、相同数字 3 张

4、相同数字 4 张

5、相同数字 3 张 + 相同数字 2 张

6、连续 5 个及 5 个以上的数字

**分析：**

依次遍历 1 << 13 内的状态，check() 看看这个状态的牌能不能一次打出，能的话就放进 vector 中，然后 dp，

其中 dp[st] 表示牌的状态为 st 时最少需要几手打出。

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
#include <utility>
#include <map>

using namespace std;

typedef long long int ll;
typedef pair<int, int> PII;
const int MOD = (int)1e9 + 7;
const int INF = 1000000007;
const ll INFLL = 0x3f3f3f3f3f3f3f3f;

struct Node
{
	int num;
	char c;
	void print()
	{
		if (num == 14)
			printf("A");
		else if (num >= 2 && num <= 9)
			printf("%d", num);
		else if (num == 10)
			printf("T");
		else if (num == 11)
			printf("J");
		else if (num == 12)
			printf("Q");
		else
			printf("K");

		printf("%c", c);
	}
};

int t;
Node a[15];
int vis[15];
int dp[1 << 13];
int pre[1 << 13];
int top;
int Stack[20];
int all = 1 << 13;
vector<int> vec;

void print(int st)
{
	for (int i = 0; i < 13 && st; i++)
	{
		if (st & 1)
		{
			a[i].print();
			
			(st >> 1) ? printf(" ") : printf("\n");
		}
		
		st >>= 1;
	}
}

void count_bits(int st)
{
	top = 0;
	for (int i = 0; i < 13 && st; i++)
	{
		if (st & 1)
			Stack[top++] = i;
		st >>= 1;
	}
}

int check(int st)
{
	count_bits(st);
	memset(vis, 0, sizeof(vis));

	int num = 0;
	int minn = 15, maxx = 0;
	for (int i = 0; i < top; i++) // top = st状态有多少张牌
	{
		if (vis[a[Stack[i]].num] == 0)
			num++; // num = st状态有多少种牌

		vis[a[Stack[i]].num]++;
		minn = min(minn, a[Stack[i]].num);
		maxx = max(maxx, a[Stack[i]].num);
	}

	if (num == 1 && top <= 4) // 可打单张，或一对，或三个头，或四个头
		return 1;
	if (num == 2 && top == 5)
	{
		if (vis[minn] == 4 || vis[maxx] == 4)
			return -1;
		return 1; // 可打三带二
	}
	if (top >= 5 && num == top && (num == maxx - minn + 1))
		return 1; // 可打顺子

	return -1;
}

int dfs(int st)
{
	if (dp[st] != -1)
		return dp[st];

	dp[st] = INF;
	for (int i = 0; i < vec.size(); i++)
	{
		int sub = vec[i];
		if ((st & sub) == sub)
		{
			int t = dfs(st ^ sub) + 1;
			if (dp[st] > t)
			{
				dp[st] = t;
				pre[st] = st ^ sub;
			}
		}
	}

	return dp[st];
}

int main()
{
	scanf("%d", &t);
	while (t--)
	{
		for (int i = 0; i < 13; i++)
		{
			char s[5];
			scanf("%s", s);
			if (s[0] == 'A')
				a[i].num = 14;
			else if ('2' <= s[0] && s[0] <= '9')
				a[i].num = s[0] - '0';
			else if (s[0] == 'T')
				a[i].num = 10;
			else if (s[0] == 'J')
				a[i].num = 11;
			else if (s[0] == 'Q')
				a[i].num = 12;
			else
				a[i].num = 13;

			a[i].c = s[1];
		}

		dp[0] = 0;
		vec.clear();

		for (int i = 1; i < all; i++)
		{
			dp[i] = check(i);
			if (dp[i] == 1)
				vec.push_back(i);
		}
		memset(pre, -1, sizeof pre);

		printf("%d\n", dfs(all - 1));

		int st = all - 1;
		while (1)
		{
			if (pre[st] == -1)
			{
				print(st);
				break;
			}
			else
				print(st - pre[st]);

			st = pre[st];
		}
	}
	return 0;
}
```
