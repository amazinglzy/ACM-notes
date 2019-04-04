[题目链接](https://www.hackerrank.com/challenges/towers)
【矩阵快速幂】

##题目大意
求一个数用给定一些数的划分数。这些数可以用无限次。
如：
$5 = 1 + 2 + 2, \ 5 = 2 + 1 + 2, \ 5 = 2 + 2 + 1$
$5 = 1 + 1 + 1 + 2$
$5 = 1 + 1 + 1 + 1 + 1$

##思路
根据题目意思可以写出递推关系出来。
可以先算出$15$以内的情况。然后用矩阵快速幂。

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;
typedef long long ll;
const int MOD = 1e9 + 7;

int a[15];

int dp[16];


ll A[16][16], B[16][16], C[16][16];
void mul(ll x[16][16], ll y[16][16]) {
	memset(C, 0, sizeof(C));
	for (int i = 0; i < 16; i++) {
		for (int j = 0; j < 16; j++) {
			for (int k = 0; k < 16; k++) {
				C[i][j] += x[i][k] * y[k][j];
				C[i][j] %= MOD;
			}
		}
	}
	for (int i = 0; i < 16; i++) {
		for (int j = 0; j < 16; j++) {
			x[i][j] = C[i][j];
		}
	}
}

void pow(ll x[16][16], ll n) {
	memset(B, 0, sizeof(B));
	for (int i = 0; i < 16; i++) B[i][i] = 1;

	while ( n ) {
		if ( n & 1 ) mul(B, x);
		mul(x, x);
		n >>= 1;
	}

	for (int i = 0; i < 16; i++) {
		for (int j = 0; j < 16; j++) {
			x[i][j] = B[i][j];
		}
	}
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	ll n;
	int k;
	cin >> n >> k;
	for (int i = 0; i < k; i++) {
		scanf("%d", &a[i]);
	}

	dp[0] = 1;
	for (int i = 1; i <= 15; i++) { for (int j = 0; j < k; j++) { if (i>= a[j]) {
				dp[i] += dp[i - a[j]];
			}
		}
	}

	/*
	for (int i = 0; i <= 15; i++) { printf("%d %d\n", i, dp[i]); } */ if (n> 15) {
		memset(A, 0, sizeof(A));
		for (int i = 0; i < 15; i++) {
			A[i + 1][i] = 1;
		}
		for (int i = 0; i < k; i++) {
			A[0][a[i] - 1] = 1;
		}

		/*
		for (int i = 0; i < 16; i++) {
			for (int j = 0; j < 16; j++) {
				cout << A[i][j] << ' ';
			}
			cout << endl;
		}

		cout << endl;
		*/

		pow(A, n - 15);

		/*
		for (int i = 0; i < 16; i++) {
			for (int j = 0; j < 16; j++) {
				cout << A[i][j] << ' ';
			}
			cout << endl;
		}
		*/

		ll ans = 0;
		for (int i = 0; i < 16; i++) {
			ans = (ans + A[0][i] * dp[15 - i]) % MOD;
		}
		ans = 2 * ans % MOD;
		cout << ans << endl;
	} else {
		cout << 2 * dp[n] % MOD << endl;
	}

	return 0;
}
```
