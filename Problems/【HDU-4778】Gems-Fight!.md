[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=4778)

##题目大意
* 有$G$种颜色的宝石
* $B$堆上面颜色的宝石
* 两个人轮流取一堆
* 取走都放在同一个地方
* 这个地方每$S$个相同颜色的宝石就合成一个$Magic Stone$
* 当前放置的人再将这些$Magic Stone$取走
* 然后这个人获得再取一堆的机会
* 如果放置之后再合成$Magic Stone$仍有再取一堆的机会
* 如果可能，可以一直取下去
* 最后输出每个人的$Magic Stone$数目差
* $(0 \leq B \leq 21, 0 \leq G \leq 8, 0 \lt n \leq 10, S \lt 20)$


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
typedef pair<ll, int> P;
typedef unsigned int ui;

const int N = 1100;
const int INF = 0x3f3f3f3f;

const int M = 21;
int res[1<<M][10];
int cnt[M][10];
int b, g, n, s;
int vis[1<<M];
void init(ll bs) {
    if (vis[bs]) return;
    vis[bs] = true;
    for (int i = 0; i < b; i++) {
        if (!((bs>> i) & 1) ) {
			for (int j = 0; j < 10; j++) {
				res[bs | (1 << i)][j] = (res[bs][j] + cnt[i][j]) % s;
			}
			init(bs | (1 << i));
		}
	}
}

int dp[1<<M];
int dfs(int bs) {
	if (dp[bs]>= 0) return dp[bs];
	dp[bs] = -INF;

	bool update = false;
	for (int i = 0; i < b; i++) {
		int add = 0;
		if ((bs >> i) & 1) continue;
		update = true;
		for (int j = 0; j < 10; j++) {
			add += (res[bs][j] + cnt[i][j]) / s;
		}
		if (add) {
			dp[bs] = maxv(dp[bs], dfs(bs | (1 << i)) + add);
		} else {
			dp[bs] = maxv(dp[bs], -dfs(bs | (1 << i)));
		}
	}

	if (!update) dp[bs] = 0;
	return dp[bs];
}

int main() {
//    freopen("in.txt", "r", stdin);
//    freopen("out.txt", "w", stdout);

	while ( scanf("%d%d%d", &g, &b, &s), b||g||s ) {
		for (int i = 0; i < (1 << b) ; i++) {
			for (int j = 0; j < g; j++) {
				res[i][j] = 0;
			}
		}

		memset(cnt, 0, sizeof(cnt));

		int t1;
		for (int i = 0; i < b; i++) {
			scanf("%d", &n);
			for (int j = 0; j < n; j++) {
				scanf("%d", &t1);
				cnt[i][t1] ++;
			}
		}

		memset(vis, 0, sizeof(vis));
		init(0);

		memset(dp, -1, sizeof(dp));
		printf("%d\n", dfs(0));
	}


	return 0;
}
```
