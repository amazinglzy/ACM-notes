[题目链接](http://www.spoj.com/problems/VECTAR5/)
【组合数学】

##题目大意
给定集合的大小，求两个集合的组合数目。两个集合不能互为子集。

##公式推导
定义$ans(x)$为所求，定义$f(x)$为给定集合大小，求不相交的子集个数。
那么：
$$ans(x) = \sum_{i = 0}^{x}C_x^if(x - i) = \sum_{i=0}^{x}C_x^if(i)$$

$$f(x) = 3 ^ k - 2 ^ {k + 1} + 1$$

$$ans(x) = \sum_{i = 0}^{x}C_x^i(3^k - 2^{k + 1} + 1)$$

$$ans(x) = 4^x - 2 \times 3^x + 2^x$$

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>

using namespace std;
typedef long long ll;

const ll MOD = 1e9 + 7;

ll pow_mod(ll a, ll n) {
	ll ret = 1;
	while ( n ) {
		if ( n & 1 ) ret = (ret * a) % MOD;
		a = (a * a) % MOD;
		n >>= 1;
	}
	return ret;
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	int t, n;
	scanf("%d", &t);
	while ( t-- ) {
		scanf("%d", &n);
		ll ans = pow_mod(4, n);
		ans = (ans - 2 * pow_mod(3, n)) % MOD;
		if ( ans < 0) ans += MOD;
		ans = (ans + pow_mod(2, n)) % MOD;
		cout << ans << endl;
	}

	return 0;
}
```
