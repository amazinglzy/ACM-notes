[题目链接](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=5054)

##题目大意
* 一个有向图
* 两人轮流在图中走，经一条边从一个点到另一个点
* 谁无法走就输了
* 两个子在一起先手胜$(YES)$
* 如果无法确定谁输谁赢，后手胜$(NO)$
* $(2 \leq n \leq 100, 1 \leq m \leq n \times (n - 1))$

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#define minv(x,y) ((x) < (y) ? (x) : (y))
#define maxv(x,y) ((x) > (y) ? (x) : (y))

using namespace std;
typedef double db;
typedef long long ll;
typedef unsigned int ui;

const int N = 200;
vector<int> g[N];

struct P {
	int x, y, k;
	P (int x, int y, int k) : x(x), y(y), k(k) { }
};
queue<P> que;
int n, m;
int out[N];
int dp[N][N][2];
int cnt[N][N][2];
int vis[N][N][2];

int main() {
//    freopen("in.txt", "r", stdin);
//    freopen("out.txt", "w", stdout);

	int t, cas = 0;
	scanf("%d", &t);
	while ( t -- ) {
		scanf("%d%d", &n, &m);

		for (int i = 1; i <= n; i++) {
			out[i] = 0;
			g[i].clear();
		}

		int u, v;
		for (int i = 0; i < m; i++) {
			scanf("%d%d", &u, &v);
			g[v].push_back(u);
			out[u] ++;
		}

		while (!que.empty()) que.pop();
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= n; j++) {
				dp[i][j][0] = 1;
				dp[i][j][1] = 1;
				vis[i][j][0] = 0;
				vis[i][j][1] = 0;
				cnt[i][j][0] = 0;
				cnt[i][j][1] = 0;

				if (i == j) {
					dp[i][j][0] = 0;
					dp[i][j][1] = 0;
					vis[i][j][0] = 1;
					vis[i][j][1] = 1;
					que.push(P(i, j, 0));
					que.push(P(i, j, 1));
				}

				if (out[j] == 0) {
					dp[i][j][1] = 0;
					que.push(P(i, j, 1));
					vis[i][j][1] = 1;
				}
			}
		}


		while (!que.empty()) {
			P p = que.front(); que.pop();
			int cx = p.x, cy = p.y, cs = p.k;
			if (cs == 0) {
				for (int i = 0; i < g[cy].size(); i++) {
					int pre = g[cy][i];
					if (vis[cx][pre][1] == 1) continue;
					cnt[cx][pre][1] ++;
					if (cnt[cx][pre][1] == out[pre]) {
						dp[cx][pre][1] = 0;
						vis[cx][pre][1] = 1;
						que.push(P(cx, pre, 1));
					}
				}
			} else {
				for (int i = 0; i < g[cx].size(); i++) {
					int pre = g[cx][i];
					if (vis[pre][cy][0] == 1) continue;
					dp[pre][cy][0] = 0;
					vis[pre][cy][0] = 1;
					que.push(P(pre, cy, 0));
				}
			}
		}

		int x, y;
		scanf("%d%d", &x, &y);
		printf("Case #%d: %s\n", ++cas, dp[y][x][1] == 1 ? "Yes" : "No");
	}


	return 0;
}
```
