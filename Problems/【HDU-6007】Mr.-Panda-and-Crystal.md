[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6007)
## 题目大意
有 $n$ 种水晶，每一种水晶价值为 $p_i$，现有 $M$ 的魔力，对于 $n$ 种水晶的一些，可以用魔力 $c_i$ 来制造，同时，可以用一些水晶来制造出其它水晶．求能够产生的最大的价值是多少？

## 限制条件
$1 \leq M \leq 10000$
$1 \leq n \leq 200$
$1 \leq c_i, p_i \leq 10000$

## 思路
对于每种水晶，求出生产它所要的最少的魔力，就可以用一个多重背包来求对于魔力 $M$ 可以生产的最大价值．
现关键在于如何求出生产某种水晶所需要最少的魔力，一个水晶的生产所需的魔力，取决于生成这个水晶的魔力（能够直接生成）和通过其它公式合成，如果它是一个树型结构，那么直接扫一遍就可以了，但题目没有说，也就是说，生成关系中是会存在相互依赖的关系的．考虑求一个图的最短路，对于一个点的最短路的更新，必会影响从这个点转移的点的最短路，所以这个题的模型比较像最短路模型．

## 代码实现
```cpp
# include <bits/stdc++.h>

using namespace std;
# define cmin(x,y) ((x)=min(x,y))
# define cmax(x,y) ((x)=max(x,y))
# define sq(x) ((x)*(x))
# define x first
# define y second
# define show(x) cerr<<#x<<" = "<<x<<endl typedef pair<int, int> Pr;
typedef double DB;
typedef long long LL;
const int N = 300;
const int M = 1e4 + 100;

int m, n, k;
int cost[N], val[N];

vector<Pr> g[N];
int to[N];

vector<int> rel[N];
queue<int> que;
bool vis[N];
void spfa() {
    while (!que.empty()) {
        int i = que.front(); que.pop();
        int tmp = 0;
        int u = to[i];
        for (int j = 0; j < g[i].size(); j++) {
            tmp += cost[g[i][j].x] * g[i][j].y;
        }
        if (cost[u] > tmp) {
            cost[u] = tmp;
            for (int j = 0; j < rel[u].size(); j++) if (!vis[rel[u][j]])
                que.push(rel[u][j]), vis[rel[u][j]] = true;
        }
        vis[i] = false;
    }
}

int dp[M];

int main(void) {
#ifdef owly
    freopen("a.in", "r", stdin);
    //freopen("a.out", "w", stdout);
#endif // owly

    int T, cnt = 1;
    scanf("%d", &T);
    while (T --) {
        scanf("%d%d%d", &m, &n, &k);
        for (int i = 0; i < k; i++)
            g[i].clear();
        for (int i = 1; i <= n; i++) {
            rel[i].clear();
            int ty;
            scanf("%d", &ty);
            if (ty == 1)
                scanf("%d%d", &cost[i], &val[i]);
            else
                scanf("%d", &val[i]), cost[i] = m + 1;
        }

        while (!que.empty())
            que.pop();
        memset(vis, 0, sizeof(vis));

        int y, u, v;
        for (int i = 0; i < k; i++) {
            scanf("%d%d", &to[i], &y);
            for (int j = 0; j < y; j++) {
                scanf("%d%d", &u, &v);
                g[i].push_back(Pr(u, v));
                rel[u].push_back(i);

                if (cost[u] <= m && !vis[i])
                    que.push(i), vis[i] = true;
            }
        }

        spfa();

        memset(dp, 0, sizeof(dp));
        for (int i = 1; i <= n; i++) if (cost[i] <= m) {
            for (int j = cost[i]; j < M; j++) {
                cmax(dp[j], dp[j - cost[i]] + val[i]);
            }
        }

        printf("Case #%d: %d\n", cnt++, dp[m]);
    }


    return 0;
}

```
