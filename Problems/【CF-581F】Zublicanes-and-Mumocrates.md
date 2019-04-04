[题目链接](http://codeforces.com/contest/581/problem/F)
【树形dp】

##题目大意
给一棵树，将树的节点分成两部分。
一部分的叶子和另一部分的叶子节点一样多，并且第一部分和第二部分相连的边尽可能的少。
求这个边的数量。

##思路
因为题目要求是两部分叶子节点一样多，所以将叶子节点数作为一个参数。
在这里，使用 $dp[i][j][0/1]$ 表示 $i$ 子树叶节点染色为 $1$ 的数目为 $j$ 且 $i$ 节点编号为 $0 / 1$ 的情况下 $0 - 1$ 边数目的最小值。
对于不可能出现的情况 $dp$ 值为 $INF$。
所以，对于叶子节点，$dp[v][0][0]= 0, dp[v][1][1] = 0, dp[v][0][1] = INF, dp[v][1][0] = INF$
因为如果叶节点为 $1$ ，那么不可能有 $0$ 个 $1$， 同理叶节点为 $0$ 时，也如此。
对于非叶节点，每加一棵他的子树，可以利用类似背包的动态规划算法。

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

#include <vector>
using namespace std;

const int N = 5e3 + 100;
const int INF = 0x3f3f3f3f;

int n;
vector<int> g[N];

int dp[N][N][2];
int dfs(int v, int fa) {
	int tot = 0, now = 0;
	dp[v][0][0] = 0;
	dp[v][0][1] = 0;

	int f = 1;
	for (int i = 0; i < g[v].size(); i ++) {
		if ( g[v][i] == fa ) continue;
		int u = g[v][i];
		f = 0;
		now = dfs(g[v][i], v);
		tot += now;
		for (int k = tot; k >= 0; k--) {
			int max0 = INF, max1 = INF;
			for (int j = 0; j <= now; j++) {
				max0 = min(max0, min(dp[v][k - j][0] + dp[u][j][0], dp[v][k - j][0] + dp[u][j][1] + 1));
				max1 = min(max1, min(dp[v][k - j][1] + dp[u][j][1], dp[v][k - j][1] + dp[u][j][0] + 1));
			}
			dp[v][k][0] = max0;
			dp[v][k][1] = max1;
		}
	}
	if ( f ) {
	//  dp[v][1][0] = INF; 因为memset过后本来就是INF，所以可以不加
		dp[v][0][1] = INF;
		dp[v][1][1] = 0;
	}
	return tot + f;
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	int a, b;
	scanf("%d", &n);
	for (int i = 0; i < n - 1; i++) {
		scanf("%d%d", &a, &b);
		g[a].push_back(b);
		g[b].push_back(a);
	}

	if ( n == 2 ) puts("1");
	else {
		int leaf = 0;
		int root = -1;
		for (int i = 1; i <= n; i++) {
			if ( g[i].size() == 1 ) leaf ++;
			else if ( root == -1 ) root = i;
		}

		memset(dp, INF, sizeof(dp));
		dfs(root, root);
		printf("%d\n", min(dp[root][leaf/2][0], dp[root][leaf/2][1]));

	}

	return 0;
}
```
