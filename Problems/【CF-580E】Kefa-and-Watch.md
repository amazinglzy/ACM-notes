[题目链接](http://codeforces.com/contest/580/problem/E)
【字符串hash】【线段树】【周期性判定】

##题目大意
给一个字符串s。
两种操作。
一种是对[l, r]区间的字符修改成c。
另一种是查询[l, r]区间的字符是否有周期性。

##思路
首先，判断一个字符串$[l, r]$是否具有周期性$d$，只用判断$[l+d, r]$和$[l, r-d]$是否一样就行。容易通过画图看出来。
所以用字符串hash来判断字符串是否相等。
用线段树维护每个位置的hash值及其和。

##代码实现
```cpp
#include <cstdio>

using namespace std;
typedef long long ll;

const int N = 1e5 + 100;
const int B = 19;
const int MOD = 1e9 + 7;

ll bk[N], inv;

ll pow_mod(ll a, ll n) {
	ll ret = 1;
	while ( n ) {
		if (n & 1) ret = (ret * a) % MOD;
		a = (a * a) % MOD;
		n >>= 1;
	}
	return ret;
}

int n, m, k;

char s[N];
ll sum[4 * N], lazy[4 * N];

void push_down(int k, int l, int r) {
	if ( lazy[k] != -1 ) {
		sum[k] = lazy[k] * (bk[n - l] - bk[n - r - 1] + MOD) % MOD;

		if (l != r) {
			lazy[2 * k + 1] = lazy[k];
			lazy[2 * k + 2] = lazy[k];
		}
		lazy[k] = -1;
	}
}

void push_up(int k) {
	sum[k] = sum[2 * k + 1] + sum[2 * k + 2];
}

void build(int k, int l, int r) {
	lazy[k] = -1;
	if (l == r) {
		sum[k] = (s[l] - '0') * (bk[n - l] - bk[n - r - 1] + MOD) % MOD;
		return;
	}
	build(2 * k + 1, l, (l + r) / 2);
	build(2 * k + 2, (l + r) / 2 + 1, r);
	push_up(k);
}

void update(int k, int l, int r, int c, int a, int b) {
	push_down(k, l, r);
	if ( a <= l && r <= b ) {
		lazy[k] = c;
		push_down(k, l, r);
		return;
	}
	if ( r < a || b < l ) return;
	update(2 * k + 1, l, (l + r) / 2, c, a, b);
	update(2 * k + 2, (l + r) / 2 + 1, r, c, a, b);

	push_up(k);
}

ll query(int k, int l, int r, int a, int b) {
	push_down(k, l, r);
	if (a <= l && r <= b) return sum[k];
	if (r < a || b < l ) return 0;
	ll left = query(2 * k + 1, l, (l + r) / 2, a, b);
	ll right = query(2 * k + 2, (l + r) / 2 + 1, r, a, b);
	return (left + right) % MOD;
}

ll hash(int l, int r) {
	ll ret = query(0, 0, n-1, l, r);
	ll div = pow_mod(inv, n - r);
	return ret * div % MOD;
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	inv = pow_mod(B, MOD - 2);

	bk[1] = 1;
	for (int i = 2; i < N; i++) bk[i] = bk[i - 1] * B % MOD;
	for (int i = 1; i < N; i++) bk[i] = (bk[i] + bk[i - 1]) % MOD;


	scanf("%d%d%d", &n, &m, &k);
	scanf("%s", s);

	build(0, 0, n - 1);

	int cm, l, r, d;
	for (int i = 0; i < m + k; i++) {
		scanf("%d%d%d%d", &cm, &l, &r, &d);
		l--; r--;
		if ( cm == 1 ) {
			update(0, 0, n-1, d, l, r);
		} else {
			if (l - r + 1 == d) {
				puts("YES");
			} else if (hash(l, r - d) == hash(l + d, r)) {
				puts("YES");
			} else {
				puts("NO");
			}
		}
	}


	return 0;
}
```
