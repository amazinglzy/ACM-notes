[题目链接](https://asiahk16prel.kattis.com/problems/boxes)
【LCA】

##题目大意
一个森林，询问k个节点下有多少个节点？

##思路
求$k$个节点最上的节点集合，也就是这样节点对应的子树不相交。
将这些节点对应的子树的节点数加上就好。
可以用$LCA$来判断是子树是否相交。

##代码实现
```cpp
#include <cstdio>
#include <cmath>
#include <vector>
#include <algorithm>

using namespace std;
typedef pair<int, int> P;

const int N = 2e5 + 100;

int pa[N];
int find(int x) {
	return x == pa[x] ? x : pa[x] = find(pa[x]);
}

int n, q;
vector<int> g[N];
int sz[N];

void getsz(int v) {
	sz[v] = 1;
	for (int i = 0; i < (int)g[v].size(); i++) {
		getsz(g[v][i]);
		sz[v] += sz[g[v][i]];
	}
}

int first[N], deps[2*N], id[2*N], tot;
void dfs(int v, int dep) {
	first[v] = tot;
	deps[tot] = dep;
	id[tot ++] = v;
	for (int i = 0; i < (int)g[v].size(); i++) {
		dfs(g[v][i], dep + 1);
		deps[tot] = dep;
		id[tot++] = v;
	}
}

P st[21][2*N];
int lca(int u, int v) {
	int l = first[u];
	int r = first[v];
	if ( l > r ) swap(l, r);
	if ( l == r ) return st[0][l].second;
	int k = log2(r - l + 1);
	P a = st[k][l], b = st[k][r - (1 << k) + 1]; if ( a.first> b.first ) return b.second;
	else return a.second;
}

int fa[N];
int main(void) {
//	freopen("in.txt", "r", stdin);

	scanf("%d", &n);
	for (int i = 0; i <= n; i++)
	    pa[i] = i;
	for (int i = 1; i <= n; i++) {
	    scanf("%d", &fa[i]);
	    if ( fa[i] ) {
	        g[fa[i]].push_back(i);
	        pa[i] = fa[i];
	    }
	}
	tot = 0;
	for (int i = 1; i <= n; i++) {
	    if ( ! fa[i] ) {
	        getsz(i);
	        dfs(i, 0);
	    }

    }
    for (int i = 0; i < tot; i++) {
        st[0][i].first = deps[i];
        st[0][i].second = id[i];
    }
    for (int i = 1; i < 21; i++) {
        for (int j = 0; j + (1 << i) < tot; j++) {
            if (st[i - 1][j].first < st[i - 1][j + (1 << (i - 1))].first) {
                st[i][j] = st[i - 1][j];
            } else {
                st[i][j] = st[i - 1][j + (1 << (i - 1))];
            }
        }
    }
    int m, num, tmp;
    scanf("%d", &m);
    while ( m-- ) {
        scanf("%d", &num);
        scanf("%d", &tmp);
        vector<int> tops;
		tops.push_back(tmp);
		for (int i = 1; i < num; i ++) {
			scanf("%d", &tmp);

			for (vector<int>::iterator it = tops.begin(); it != tops.end();) {
				if ( find(tmp) != find(*it) ) {
					it ++;
					continue;
				}
				int common = lca(tmp, *it);
				if ( common == *it ) {
					tmp = *it;
					tops.erase(it);
				} else if ( common == tmp ) {
					tops.erase(it);
				} else {
					it ++;
				}
			}
			tops.push_back(tmp);
		}


		int ans = 0;
		for (int i = 0; i < tops.size(); i++) {
			ans += sz[tops[i]];
		}

		printf("%d\n", ans);
	}

	return 0;
}
```
