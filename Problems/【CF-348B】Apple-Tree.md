[题目链接](http://codeforces.com/contest/348/problem/B)

##题目大意
* 给一棵树
* 每个叶节点有个权值
* 能将某叶节点的权值减少
* 一棵子树的权值是其所有的叶节点的权值和
* 当树的每个节点的对应所有子树的权值相等是，树是平衡的
* 求减少最少的权值将树变为平衡的


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

const int N = 1e6 + 100;
const ll INF = 0x3f3f3f3f3f3f3f3f;

vector<int> g[N];

ll weight[N], n;
ll step[N];

ll lcm(ll a, ll b) { return a / __gcd(a, b) * b; }
ll ans;

bool dfs(int v, int fa) {
	if (g[v].size() == 1 && v != 1) return true;

	ll cstep = 1;
	ll min_weight = INF;
	for (int i = 0; i < g[v].size(); i++) {
		if (g[v][i] == fa) continue;

		if ( ! dfs(g[v][i], v) ) return false;
		min_weight = minv(min_weight, weight[g[v][i]]);
	}
	for (int i = 0; i < g[v].size(); i++) {
		if (g[v][i] == fa) continue;
		cstep = lcm(cstep, step[g[v][i]]);
		if (cstep > min_weight) return false;
	}

	min_weight = cstep * (min_weight / cstep);;
	for (int i = 0; i < g[v].size(); i++) {
		if (g[v][i] == fa) continue;
		ans += weight[g[v][i]] - min_weight;
	}
	step[v] = cstep * (g[v].size() - 1);
	weight[v] = min_weight * (g[v].size() - 1);
	return true;
}

int main() {
//    freopen("in.txt", "r", stdin);
//    freopen("out.txt", "w", stdout);

    ll sum = 0;
	scanf("%I64d", &n);
	for (int i = 1; i <= n; i++) {
		scanf("%I64d", &weight[i]);
		sum += weight[i];
	}

	int u, v;
	for (int i = 1; i < n; i++) {
		scanf("%d%d", &u, &v);
		g[u].push_back(v);
		g[v].push_back(u);
	}

	for (int i = 1; i <= n; i++) {
		step[i] = 1;
	}

    ans = 0;
    bool ok = dfs(1, 1);

    /*
    for (int i = 1; i <= n; i++) {
    	printf("%d ", weight[i]);
    }
    printf("\n");
    for (int i = 1; i <= n; i++) {
    	printf("%d ", step[i]);
    }
    printf("\n");
    */
	if ( ! ok ) cout << sum << endl;
	else cout << ans << endl;    

    return 0;
}
```
