[题目链接](http://codeforces.com/contest/498/problem/B)

##题目大意
* $n$首歌，一个人按顺序播放，另一个人来认歌的名字
* 每首歌有一个$t_i$，表示如果播放到$T$秒，这首歌被认出来了
* 每首歌有一个概率，表示在$t_i$内这首歌在没有被认出时听$1$秒被认出的概率
* 在给定时间的情况下，求被认出的歌的数量的期望
* $(1 \leq n \leq 5000, 1 \leq T \leq 5000), (0 \leq q \leq 100, 1 \leq t_i \leq T)$

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

const int N = 5e3 + 100;
const int INF = 0x3f3f3f3f;

db f[N][N];
int n, m;

int main() {
//    freopen("in.txt", "r", stdin);
//    freopen("out.txt", "w", stdout);

	scanf("%d%d", &n, &m);
	f[0][0] = 1;

	db s = 0;
	int x, y;
	for (int i = 1; i <= n; i++) {
	    scanf("%d%d", &x, &y);
	    db p = x / 100.0, q = 0, r = 1, t = 0;
	    for (int j = 1; j <= y; j++)
	        q = q + p * r, r = r * (1 - p);
	    for (int j = 1; j <= m; j++) {
	        t = t * (1 - p) + f[i - 1][j - 1] * p;
	        if (j>= y) t = t + f[i - 1][j - y] * (1 - q);
			if (j > y) t = t - f[i - 1][j - y - 1] * r;
			f[i][j] = t;
			s = s + f[i][j];
		}
	}
	printf("%.12lf\n", s);

	return 0;
}

```
