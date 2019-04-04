[比赛链接](http://codeforces.com/contest/743)
#A : Vladik and flights

**tags** : 构造

**description** ：

n个飞机场位置为1， 2， 3， ...，属于两家公司，属于同一家公司的飞机场之间通行免费，不同公司则它们的位置差收费。

```cpp
#include <cstdio>

using namespace std;

const int N = 1e5 + 10;
char s[N];

int main(void) {
//    freopen("in.txt", "r", stdin);

    int n, a, b;
    scanf("%d%d%d", &n, &a, &b);
    scanf("%s", s);
    printf("%d\n", s[a - 1] == s[b - 1] ? 0 : 1);

    return 0;
}
```

#B : Chloe and the sequence
**tags** : 构造

**description** ：

一个数列，它的构造方法是：
首先只有一个元素 1 。
将数列和一个数（当前该环节次数 + 1， 即2， 3， 4， ...）以 数列 数 数列 三部分的顺序组成新数列，连续进行n-1次。
求该数列第k个数是多少。

**solution** :

可以反向思考，对数列拆分。
相当于将当前数列拆成三部分，我们可以知道中间的数，要求的数的位置，如果在后半部分，可以转化成前半部分（因为是相同的）。

```cpp
#include <cstdio>
#include <iostream>

using namespace std;
typedef long long LL;


int main(void) {
//    freopen("in.txt", "r", stdin);

    LL n, k;
    cin >> n >> k;

    LL o;
    int ans = 0;
    for (int i = n - 1; i >= 0; i--) {
        o = 1;
        o = (o << i);
        if (o == k) {
            ans = i + 1;
            break;
        } else if (k < o) {
            continue;
        } else {
            k -= o;
        }
    }
    printf("%d\n", ans);
    return 0;
}

```

#C : Vladik and fraction
**tags** : 构造

**description** : 给一个数n，找三个数a, b, c使 $ \frac{2}{n} = \frac{1}{a} + \frac{1}{b} + \frac{1}{c}$

**solution** : $ \frac{2}{n} = \frac{1}{n} + \frac{1}{n+1} + \frac{1}{n(n+1)} $ 需要特判n为1的情况。

```cpp
#include <cstdio>

using namespace std;

int main(void) {
//    freopen("in.txt", "r", stdin);

    int a;
    scanf("%d", &a);

    if (a != 1) printf("%d %d %d\n", a, a + 1, a * (a + 1));
    else printf("-1\n");

    return 0;
}
```

#D : Chloe and pleasant prizes
**tags** : 树形dp

**description** :

给一棵树，每个点有一个权值，找两棵互不相交的子树使权值和最大，权值有正有负

**solution** :

第一次dfs处理出各子树的权值和，第二次dfs求出子树下的最大权值子树。
最后递归扫一遍root的子节点，答案在各子树最大和次大之和 和 各子树最大形成的最大和次大之和 中选一个最大的。

```cpp
#include <cstdio>
#include <vector>
#include <iostream>

using namespace std;
typedef long long LL;

const int N = 2e5 + 100;
const LL INF = 2e15;
vector<int> g[N];

int n;
int f[N];
LL fs[N];
void getf(int u, int fa) {
    fs[u] = f[u];
    for (int i = 0; i < (int)g[u].size(); i++) {
        if ( g[u][i] == fa ) continue;
        getf(g[u][i], u);
        fs[u] += fs[g[u][i]];
    }
}

LL maxf[N], nextf[N];
void find(int u, int fa) {
    maxf[u] = fs[u];
    for (int i = 0; i < (int)g[u].size(); i++) {
        if (g[u][i] == fa) continue;
        find(g[u][i], u);
        if ( maxf[g[u][i]] > maxf[u]) {
            nextf[u] = maxf[u];
            maxf[u] = maxf[g[u][i]];
        } else if ( maxf[g[u][i]] > nextf[u] ) {
            nextf[u] = maxf[g[u][i]];
        }

        if ( nextf[g[u][i]] > nextf[u] ) {
            nextf[u] = nextf[g[u][i]];
        }
    }
}

LL getans(int u, int fa) {
    LL maxfv = -INF, nextfv = -INF;
    for (int i = 0; i < (int)g[u].size(); i++) {
        if ( g[u][i] == fa ) continue;
        if ( maxf[g[u][i]] > maxfv ) {
            nextfv = maxfv;
            maxfv = maxf[g[u][i]];
        } else if ( maxf[g[u][i]] > nextfv ) {
            nextfv = maxf[g[u][i]];
        }
    }
    LL ans = (nextfv == -INF ? -INF : maxfv + nextfv);

    for (int i = 0; i < (int)g[u].size(); i++) {
        if ( g[u][i] == fa ) continue;
        ans = max(ans, getans(g[u][i], u));
    }
    return ans;
}

int main(void) {
//    freopen("in.txt", "r", stdin);

    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &f[i]);
    }
    int u, v;
    for (int i = 0; i < n - 1; i++) {
        scanf("%d%d", &u, &v);
        g[u].push_back(v);
        g[v].push_back(u);
    }
    getf(1, 1);
    for (int i = 0; i <= n; i++) {
        maxf[i] = -INF; nextf[i] = -INF;
    }
    find(1, 1);
    // for (int i = 1; i <= n; i++) {
        // cout << fs[i] << endl;
        // cout << maxf[i] << '\t' << nextf[i] << endl;
    // }
    LL ans = getans(1, -1);
    if (ans == -INF)
        printf("Impossible\n");
    else
        cout << ans << endl;
    return 0;
}
```

#E : Vladik and cards
**tags** : bitmask dp 二分
**description** : 给定一个序列，求满足条件子序列的最大长度。 条件1：1-8中每个数个数差不超过1 条件2：想同的数必需连续 **solution** : 二分每个数至少出现的次数，那么每个数出现次数就两种情况，用dp求出可能的最长字符串 使用mask，每一位表示一个数是否使用过，状态方程为： dp[mask][pos] 指pos及pos以后子序列生成mask的子序列的最大长度 dp[mask][pos] = max{dp[mask'][pos'] + k, dp[mask][pos+1]} (k为两种可能的次数) mask' 是指 mask 某0位置1生成 pos' 是指 pos 向后找到(k-1)个相同的数的位置
```cpp

#include <cstdio>
#include <vector>
#include <iostream>

using namespace std;

const int N = 1e3 + 100;
vector<int> v[N];
int a[N], n;
int wh[N];

int k;
int dp[300][N];
int dfs(int mask, int pos) {
    if ( mask == 255 ) return 0;
    if ( pos >= n ) {
        if (k == 0) return 0;
        else return -1;
    }
    if ( dp[mask][pos] >= -1 ) return dp[mask][pos];

    int num = a[pos];
    int ret = dfs(mask, pos + 1);
    if ( !(mask & (1 << num)) ) {
        if ( k != 0 ) {
            if ( wh[pos] + k - 1 < (int)v[num].size()) {
                int nextpos = v[num][wh[pos] + k - 1] + 1;
                int dd = dfs((mask | (1 << num)), nextpos);
                if (dd != -1) ret = max(ret, dd + k);
            }
        } else {
            int dd = dfs((mask | (1 << num)), pos);
            if (dd != -1) ret = max(ret, dd);
        }
        if ( wh[pos] + k < (int)v[num].size() ) {
            int nextpos = v[num][wh[pos] + k] + 1;
            int dd = dfs((mask | (1 << num)), nextpos);
            if (dd != -1)
                ret = max(ret, dd + k + 1);
        }
    }
    return dp[mask][pos] = ret;
}

int ok(int l) {
    k = l;
    for (int i = 0; i < 300; i++) {
        for (int j = 0; j < n; j++) {
            dp[i][j] = -2;
        }
    }
    return dfs(0, 0);
}

int main(void) {
    // freopen("in.txt", "r", stdin);
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
        a[i] --;
        v[a[i]].push_back(i);
        wh[i] = v[a[i]].size() - 1;
    }
    int ans = 4;
    int left = 0, right = v[0].size();
    for (int i = 1; i < 8; i++)
        right = max(right, (int)v[i].size());
    while ( right - left> 1 ) {
        int mid = (left + right) / 2;
        ans = ok(mid);
        if ( ans == -1 ) right = mid;
        else left = mid;
    }

    printf("%d\n", ok(left));

    return 0;
}
```
