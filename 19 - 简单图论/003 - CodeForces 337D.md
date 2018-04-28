[https://cn.vjudge.net/problem/CodeForces-337D](https://cn.vjudge.net/problem/CodeForces-337D)

**题意：**

对于一个 n 个点的树, 其中有 m 个点出现了恶魔, 并给出 m 个点的编号，
现在已知这些恶魔出现的原因是有一本恶魔之书造成的, 而且书到所有恶魔出现的点的距离不能超过d, 
问这棵树中有哪些点可能是恶魔之书出现的位置, 输出可能的位置的数量。

**分析：**

把这 m 个点看成一棵子树，求出它的最大直径（端点分别为 A 和 B）。那么可以推出以下定理：

若某点到 A 和 B 的距离都不超过 d，那么这个点到这棵子树上的所有点的距离也不超过 d。

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
#include <set>
using namespace std;
typedef long long int ll;
typedef ll LL;
typedef pair<int, int> PII;
const int MOD = 100000;
const int INF = 1000000007;
const ll INFLL = 0x3f3f3f3f3f3f3f3f;

int n, m, d;  
vector <int> vec[100010];  
int d1[100010], d2[100010];  
int p[100010];  
  
int bfs(int start)  
{  
    queue <int> Q;  
    memset(d1, -1, sizeof(d1));  
    d1[start] = 0;  
    Q.push(start);  
    while(!Q.empty())  
    {  
        int now = Q.front();  
        Q.pop();  
        for(unsigned int i = 0, sz = vec[now].size(); i < sz; i++)  
            if(d1[vec[now][i]] == -1)  
                d1[vec[now][i]] = d1[now] + 1, Q.push(vec[now][i]);  
    }  

    int ret = 1;  
    for(int i = 1; i <= m; i++)  
        if(d1[p[i]] > d1[p[ret]]) ret = i;  

    return p[ret];  
}  
  
int main()  
{  
	scanf("%d %d %d", &n, &m, &d);

    for(int i = 1; i <= m; i++)  
        scanf("%d", p + i);

    int u, v;
    for(int i = 1; i < n; i++)
    {  
        scanf("%d %d", &u, &v);  
        vec[u].push_back(v);  
        vec[v].push_back(u);  
    }

    int A = bfs(p[1]); // A 是直径的一端
    int B = bfs(A); // B 是直径的另外一端

	// d2[i] 为点 i 到 A 的距离
    for(int i = 1; i <= n; i++)
        d2[i] = d1[i];

	// d1[i] 为点 i 到 B 的距离  
    bfs(B);  
    
    int ans = 0;  
    for(int i = 1; i <= n; i++)  
        if(d1[i] <= d && d2[i] <= d)  
            ans++;

    printf("%d\n", ans);

    return 0;  
}  
```
