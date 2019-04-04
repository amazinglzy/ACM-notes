[题目链接](https://open.kattis.com/problems/lockedtreasure)
【组合数学】

##题目大意
有一张门，要打开它需要有它上面的所有的锁的钥匙。每个人都有一些钥匙，要求$n$个人中至少需要$m$个人才能将门打开时，锁的个数最少是多少。

##代码实现
```cpp
#include <cstdio>
#include <cstring>

using namespace std;

const int N = 50;
int dp[N][N];

int f(int n, int m) {
	if ( m > n) return 0;
	if ( m == 1) return n;
	if ( m == 0) return 1;
	if ( dp[n][m] >= 0) return dp[n][m];
	return dp[n][m] = f(n - 1, m) + f(n - 1, m - 1);
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	memset(dp, -1, sizeof(dp));
	int t, n, m;
	scanf("%d\n", &t);
	while ( t-- ) {
		scanf("%d%d", &n, &m);
		printf("%d\n", f(n, m - 1));
	}


	return 0;
}
```
