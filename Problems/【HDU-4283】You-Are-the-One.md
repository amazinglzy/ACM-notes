[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=4283)
【区间dp】

##题目大意
一个数列$\{d_i\}$，定义
$$y = \sum_{i = 0}^{n - 1} i \times d_i$$

现有一个栈，可以通过来调整栈来调整元素的位置，$y$的最小是多少？

##思路
对于第一个元素，它可以先入栈，再在某个位置再出栈。那么，新数列的这个位置之前的几个数是最开始的数列的这个数的后几个数，只不过是顺序不同。这样就将大问题化成了子问题。
然后直接记忆化搜索 （或是区间DP）。



##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <queue>

#define minv(x,y) ((x) < (y) ? (x) : (y))
#define maxv(x,y) ((x) > (y) ? (x) : (y))
using namespace std;

const int N = 200;
const int INF = 0x3f3f3f3f;

int dp[N][N];
int d[N], n;
int sum[N];
int dfs(int l, int r) {
	if (l > r) return 0;
	if (l == r ) return l * d[l];
	if (dp[l][r] != INF) return dp[l][r];

	for (int k = l; k <= r; k++) {
		dp[l][r] = min(dp[l][r], k * d[l] + dfs(l+1, k) + dfs(k + 1, r) - sum[k + 1] + sum[l + 1]);
	}
	return dp[l][r];
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	int t, cnt = 1;
	scanf("%d", &t);
	while ( t-- ) {
		scanf("%d", &n);
		for (int i = 0; i < n; i++) scanf("%d", &d[i]);
		for (int i = 1; i <= n; i++) sum[i] = sum[i - 1] + d[i - 1];

		memset(dp, 0x3f, sizeof(dp));
		printf("Case #%d: ", cnt ++);
		printf("%d\n", dfs(0, n - 1));
	}


	return 0;
}
```
