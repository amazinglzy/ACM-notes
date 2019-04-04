[题目链接](http://codeforces.com/contest/580/problem/D)
【状压dp】

##题目大意
$n$盘菜，每盘菜有$a_i$的喜悦值。
同时，存在一些菜吃的特定顺序，如$x_i$在$y_i$在前吃，且中间没有别的菜，那么就会有$c_i$的喜悦值。
吃$k$盘菜，问最大的喜悦值是多少？

##思路
这个题是一个状压dp的题。
菜的种类不多，可以用一个整型变量来存菜是否被吃。即用一个变量来存当前菜有没有吃的状态
$dp[mask][j]$表示$mask$形容的状态下，上一盘菜是j的时候的最大喜悦值。
状态转移，对所有的菜进行枚举，发现某盘菜没有吃，就可以吃那盘菜，对$mask$和$j$修改就好。
这个过程需要用到一些位运算的技巧，可以参考代码。

这里用$dp[i][mask][j]$表示第$i$盘菜的情况。
但是由于空间不足，且$dp[i][mask][j]$只由$dp[i - 1][mask][j]$来决定，所以可以只用两维，交叉存储。这就是滚动数组。

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <iostream>

using namespace std;
typedef long long LL;

const int N = 19;
const int W = 1<<18;
LL dp[2][W][N];
int m, n, k;
int a[N];
int ex[N][N];
int main(void) {
// freopen("in.txt", "r", stdin);
    scanf("%d%d%d", &n, &m, &k);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    int u, v, cost;
    for (int i = 0; i < k; i++) {
        scanf("%d%d%d", &u, &v, &cost);
        u --; v --;
        ex[u][v] = cost;
    }
    memset(dp, -1, sizeof(dp));
    dp[0][0][n] = 0;
    for (int l = 0; l < m; l++) {
        int pre = l % 2, cur = (l + 1) % 2;
        memset(dp[cur], -1, sizeof(dp[cur]));
        for (int i = 0; i < (1 << n); i++) {
            for (int j = 0; j <= n; j++) {
                if ( dp[pre][i][j] != -1 ) {
                    for (int k = 0; k < n; k++) {
                        if ( (i>> k) & 1) {
							continue;
						} else {
							dp[cur][i | (1 << k)][k] = max(dp[cur][i | (1 << k)][k], dp[pre][i][j] + a[k] + ex[j][k]);
						}
					}
				}
			}
		}
	}

	LL ans = 0;
	for (int i = 0; i < (1 << n); i++) {
		for (int j = 0; j < n; j++) {
			ans = max(ans, dp[m%2][i][j]);
		}
	}

	cout << ans << endl;

	return 0;
}
```
