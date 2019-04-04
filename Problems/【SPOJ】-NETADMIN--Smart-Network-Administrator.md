[题目链接](http://www.spoj.com/problems/NETADMIN/)
【二分】【网络流】

##题目大意
给一个图，图中一些点到指定一点存在一些路。
求这些路中边的使用次数最少的情况下的使用次数。

##思路
使用次数相当于边的流量。
这样，二分使用次数，用网络流来判断是否可行。
建图的话先用原图，原图流量为二分的流量。再新建一个源点，从源点引入一条流量为$1$的到需要用网的房屋的边。如果流量是$k$，那么表示可行。

##代码实现
TLE 可能是邻接表用 vector 不停的申请内存，释放内存消耗了太多的时间。
```cpp
#include <cstdio>
#include <cstring>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

const int N = 1e3 + 10;
const int INF = 0x3f3f3f3f;

struct EDGE {
	int to, f, rev;
	EDGE( ) { }
	EDGE(int to, int f, int rev) : to(to), f(f), rev(rev) { }
};
vector<EDGE> g[N];
void add_edge(int u, int v, int f) {
	g[u].push_back(EDGE(v, f, (int)g[v].size()));
	g[v].push_back(EDGE(u, 0, (int)g[u].size() - 1));
}

int d[N];
bool vis[N];
queue<int> que;
void bfs(int s) {
	vis[s] = true;
	d[s] = 0;
	que.push(s);
	while ( !que.empty() ) {
		int u = que.front(); que.pop();
		for (int i = 0; i < g[u].size(); i++) {
			EDGE e = g[u][i];
			if ( e.f <= 0 ) continue;
			if ( !vis[e.to] ) {
			    vis[e.to] = true; d[e.to] = d[u] + 1; que.push(e.to);
			}
	    }
	}
}

int dfs(int v, int t, int f) {
    if (v == t)
        return f;
    int ret = 0;
    for (int i = 0; i < g[v].size() && f; i++) {
        EDGE &e = g[v][i];
        if ( e.f> 0 && d[e.to] == d[v] + 1 ) {
			int dd = dfs(e.to, t, min(f, e.f));
			if ( dd > 0 ) {
				f -= dd;
				ret += dd;
				e.f -= dd;
				g[e.to][e.rev].f += dd;
			}
		}
	}
	return ret;
}

int max_flow(int s, int t) {
	int ret = 0;
	while ( true ) {
		memset(vis, 0, sizeof(vis));
		bfs(s);
		if ( !vis[t] ) return ret;
		ret += dfs(s, t, INF);
	}
}

int n, m, k;


int h[N];
int e1[N * N], e2[N * N];

bool ok(int f) {
	for (int i = 0; i < n; i++) {
		g[i].clear();
	}
	for (int i = 0; i < m; i++) {
		add_edge(e1[i], e2[i], f);
		add_edge(e2[i], e1[i], f);
	}
	for (int i = 0; i < k; i++) {
		add_edge(0, h[i], 1);
	}
	return k == max_flow(0, 1);
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	int t;
	scanf("%d", &t);
	while ( t -- ) {
		scanf("%d%d%d", &n, &m, &k);

		for (int i = 0; i < k; i++) {
			scanf("%d", &h[i]);
		}

		for (int i = 0; i < m; i++) {
			scanf("%d%d", &e1[i], &e2[i]);
		}

		int left = 0, right = N + 1;
		while ( right - left > 1 ) {
			int mid = (left + right) / 2;
			if ( ok(mid) ) right = mid;
			else left = mid;
		}
		printf("%d\n", right);
	}

	return 0;
}
```
AC代码
```cpp
#include <cstdio>
#include <cstring>
#include <queue>
#include <algorithm>

using namespace std;

const int N = 600;
const int E = N * N * 4;
const int INF = 0x3f3f3f3f;

int n, m, k;

struct EDGE { int v, f, nxt; };
EDGE e[E];
int h[N], tot;

void add_edge(int u, int v) {
	e[tot].v = v;
	e[tot].nxt = h[u];
	h[u] = tot ++;

	e[tot].v = u;
	e[tot].nxt = h[v];
	h[v] = tot ++;
}

bool vis[N];
int d[N];
queue<int> que;
void bfs(int s) {
	d[s] = 0;
	while ( !que.empty() ) que.pop();
	vis[s] = true;
	que.push(s);
	while ( !que.empty() ) {
		int v = que.front(); que.pop();
		for (int i = h[v]; i != -1; i = e[i].nxt) {
			if ( e[i].f && !vis[e[i].v] ) {
				que.push(e[i].v);
				d[e[i].v] = d[v] + 1;
				vis[e[i].v] = true;
			}
		}
	}
}

int dfs(int v, int t, int f) {
	if ( v == t ) return f;
	int ret = 0;
	for (int i = h[v]; i != -1; i = e[i].nxt) {
		if ( e[i].f && d[e[i].v] == d[v] + 1) {
			int df = dfs(e[i].v, t, min(f, e[i].f));
			e[i].f -= df; e[i^1].f += df; ret += df; f -= df;
		}
	}
	return ret;
}

int max_flow(int s, int t) {
	int ret = 0;
	while ( true ) {
		memset(vis, 0, sizeof(vis));
		bfs(s);
		if ( !vis[t] ) return ret;
		ret += dfs(s, t, INF);
	}
}

int house[N];
bool ok(int f) {
	for (int i = 0; i < 4 * m; i += 2) {
		e[i].f = f;
	}

	for (int i = 1; i < 4 * m; i += 2) {
		e[i].f = 0;
	}

	for (int i = 4 * m; i < 4 * m + 2 * k; i++) {
		if ( i % 2 == 0 ) e[i].f = 1;
		else e[i].f = 0;
	}

	return max_flow(0, 1) == k;
}


int main(void) {
//	freopen("in.txt", "r", stdin);

	int t;
	scanf("%d", &t);
	while ( t -- ) {
		memset(h, -1, sizeof(h));
		tot = 0;

		scanf("%d%d%d", &n, &m, &k);
		for (int i = 0; i < k; i++) {
			scanf("%d", &house[i]);
		}

		int a, b;
		for (int i = 0; i < m; i++) {
			scanf("%d%d", &a, &b);
			add_edge(a, b);
			add_edge(b, a);
		}

		for (int i = 0; i < k; i++) {
			add_edge(0, house[i]);
		}

		int left = -1, right = n;
		while ( right - left > 1 ) {
			int mid = (left + right) / 2;
			if ( ok(mid) ) right = mid;
			else left = mid;
		}

		printf("%d\n", right);

	}


	return 0;
}
```
