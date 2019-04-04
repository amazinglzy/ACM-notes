[题目链接](https://www.hackerrank.com/challenges/manasa-and-combinatorics)

##题目大意
给一个数$n$，表示一个字符串有$n$个字符a，有$2n$个字符b，求有多少种组合方式使得各前缀后缀的字符b的数目大于字符a。

##思路
前$3n$个字符前缀$a$的个数大于$b$的个数的可能的个数有$C_{3n}^{n-1}$
前$3n$个字符前缀和后缀$a$的个数大于$b$的个数的可能的个数有$C_{3n}^{n-2}$
容斥就好。

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
typedef long long ll;

const int MOD = 99991;
const int N = 1e6 + 100;

ll f[N];
ll pow_mod(ll a, ll n) {
	ll ret = 1;
	while ( n ) {
		if (n & 1) ret = (ret * a) % MOD;
		a = (a * a) % MOD;
		n >>= 1;
	}
	return ret;
}

ll C(ll m, ll n) {
	ll ret = 1;
	while (m && n) {
		ll m1 = m % MOD;
		ll n1 = n % MOD;
		if (m1 < n1) return 0;
		ret = ret * f[m1] * pow_mod(f[n1] * f[m1 - n1] % MOD, MOD - 2) % MOD;
		m /= MOD;
		n /= MOD;
	}
	return ret;
}



int main(void) {
//    freopen("in.txt", "r", stdin);
    f[0] = 1;
    for (int i = 1; i < N; i++) f[i] = (f[i - 1] * i) % MOD;


    ll n;

    int t;
    scanf("%d", &t);
    while ( t-- ) {
    	cin >> n;
    	ll ans = C(3*n, n);
    	ans = (ans - 2 * C(3*n, n - 1)) % MOD;
    	if (ans < 0) ans += MOD;
    	ans = (ans + C(3*n, n - 2)) % MOD;
    	cout << ans << endl;
	}


	return 0;
}
```
