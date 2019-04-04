[题目链接](http://codeforces.com/problemset/problem/406/D)
【单调栈】【LCA】

##题目大意
在x轴上有n座山，每座山的高度可以不同。
一个人可以通过铁索从一座山顶到另一个山顶当且仅当这个山顶到另一个山顶的连线不与山相交。
现有m对人，给出每个人的位置，每个人都会爬向他能爬向的x方向最远的山顶。
当他们的路径重合，求出重合的山的编号。

##思路
因为每座山的下一个目标是固定的，所以可以形成一棵树。
这样就相当于求一棵树的两个节点的LCA。

现在问题是求出树的边。
可以参考一下凸包的求法。

##代码实现
```cpp
#include <cstdio>
#include <vector>
#include <cmath>
#include <algorithm>

using namespace std;
typedef long long LL;
typedef pair<int, int> P;

const int N = 1e5 + 100;

int n, m;
LL x[N], y[N];

int fa[N];
int stack[N], top;

vector<int> g[N];
vector<int> q[N];

bool cross(int i, int j, int k) {
	LL ax = x[j] - x[i];
	LL ay = y[j] - y[i];
	LL bx = x[k] - x[j];
	LL by = y[k] - y[j];
	return ax * by - bx * ay <= 0;
}

int shot[2 * N], deps[2 * N], tot, first[N];
void addshot(int u, int dep) {
    shot[ tot ] = u;
    first[ u ] = tot;
    deps[tot ++ ] = dep;
    for (int i = 0; i < (int)g[u].size(); i++) {
        addshot(g[u][i], dep + 1);
        shot[tot] = u;
        deps[tot ++] = dep;
    }
}

P st[21][2 * N];

void construct( ) {
    for (int i = 0; i < tot; i++) {
        st[0][i] = P(deps[i], shot[i]);
    }
    for (int i = 1; i < 21; i++) {
        for (int j = 0; j + (1 << (i - 1)) < 2 * N; j++) {
            if ( st[i - 1][j].first < st[i - 1][j + (1 << (i - 1))].first ) {
                st[i][j] = st[i - 1][j];
            } else {
                st[i][j] = st[i - 1][j + (1 << (i - 1))];
            }
        }
    }
}

int findLCA(int u, int v) {
    int left = first[u], right = first[v];
    if ( left> right) swap(left, right);
	if ( left == right ) return st[0][left].second;
	int k = log2(right - left);
	if ( st[k][left].first < st[k][right - (1 << k) + 1].first ) {
	    return st[k][left].second;
	} else {
	    return st[k][right - (1 << k) + 1].second;
	}
}

int main(void) {
// freopen("in.txt", "r", stdin);
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%I64d%I64d", &x[i], &y[i]);
    }
    x[n] = x[n - 1] + 1;
    y[n] = 0; top = 0;
    stack[top ++] = n - 1;
    for (int i = n - 2; i>= 0; i--) {
		while ( top > 1 && !cross(i, stack[top - 1], stack[top - 2]) ) top --;
		fa[i] = stack[top - 1];
		stack[top ++] = i;
	}

	/*
	for (int i = 0; i < n; i++) {
		printf("%d ", fa[i]);
	}
	printf("\n");
	*/

	for (int i = 0; i < n - 1; i++) {
		g[fa[i]].push_back(i);
	}

	addshot(n - 1, 0);

	/*
	for (int i = 0; i < tot; i++) {
		printf("%d ", shot[i]);
	}
	printf("\n");

	for (int i = 0; i < n; i++) {
		printf("%d ", first[i]);
	}
	printf("\n");
	*/

	construct();

	int a, b;
	scanf("%d", &m);
	for (int i = 0; i < m; i++) {
		scanf("%d%d", &a, &b);
		a--;
		b--;
		printf("%d ", findLCA(a, b) + 1);
	}


	return 0;
}
```

```cpp
#include <cstdio>
#include <vector>
#include <cstring>
#include <algorithm>

using namespace std;
typedef long long LL;
typedef pair<int, int> P;

const int N = 1e5 + 100;

int n, m;
LL x[N], y[N];

int fa[N];
int stack[N], top;

vector<int> g[N];
vector<P> q[N];
int l[N], r[N], ans[N];

bool cross(int i, int j, int k) {
	LL ax = x[j] - x[i];
	LL ay = y[j] - y[i];
	LL bx = x[k] - x[j];
	LL by = y[k] - y[j];
	return ax * by - bx * ay <= 0;
}
bool visit[N];
int pa[N];
int find(int x) {
    return x == pa[x] ? x : pa[x] = find(pa[x]);
}
void tarjan(int u) {
    visit[u] = true;
    for (int i = 0; i < (int)q[u].size(); i++) {
        if ( visit[q[u][i].first] ) {
            ans[q[u][i].second] = find(q[u][i].first);
        }
    }
    for (int i = 0; i < (int)g[u].size(); i++) {
        tarjan(g[u][i]); pa[g[u][i]] = u;
    }
}

int main(void) { // freopen("in.txt", "r", stdin);
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%I64d%I64d", &x[i], &y[i]);
    }
    x[n] = x[n - 1] + 1;
    y[n] = 0; top = 0;
    stack[top ++] = n - 1;
    for (int i = n - 2; i>= 0; i--) {
		while ( top > 1 && !cross(i, stack[top - 1], stack[top - 2]) ) top --;
		fa[i] = stack[top - 1];
		stack[top ++] = i;
	}

	/*
	for (int i = 0; i < n; i++) {
		printf("%d ", fa[i]);
	}
	printf("\n");
	*/

	for (int i = 0; i < n - 1; i++) {
		g[fa[i]].push_back(i);
	}

	/*
	for (int i = 0; i < tot; i++) {
		printf("%d ", shot[i]);
	}
	printf("\n");

	for (int i = 0; i < n; i++) {
		printf("%d ", first[i]);
	}
	printf("\n");
	*/

	int a, b;
	scanf("%d", &m);
	for (int i = 0; i < m; i++) {
		scanf("%d%d", &a, &b);
		a--;
		b--;
		l[i] = a;
		r[i] = b;
		if ( a == b ) ans[i] = a;
		q[a].push_back(P(b, i));
		q[b].push_back(P(a, i));
	}

	memset(visit, 0, sizeof(visit));
	for (int i = 0; i < n; i++) pa[i] = i;
	tarjan(n - 1);
	for (int i = 0; i < m; i++) {
		printf("%d ", ans[i] + 1);
	}

	return 0;
}
```
