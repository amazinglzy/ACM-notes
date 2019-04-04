[题目链接](https://www.hackerrank.com/challenges/alien-flowers)
【组合数学】

##题目大意
一个字符串由字符B,R组成，给定子串BB,BR,RB,RR的出现次数，求该字符中的可能的情况数。

##思路
考虑$B、R$的相连的块数。用挡板法就好。
根据$BB,BR,RB,RR$的个数，可以知道连续的$B,R$的个数。

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

const int MOD = 1e9 + 7;
const int N = 2e5 + 100;

int a, b, c, d;
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

ll C(int n, int m) {
	return f[n] * pow_mod(f[m] * f[n - m] % MOD, MOD - 2) % MOD;
}

int main(void) {
//    freopen("in.txt", "r", stdin);

    f[0] = 1;
    for (int i = 1; i < N; i++) f[i] = (f[i - 1] * i) % MOD;

    scanf("%d%d%d%d", &a, &b, &c, &d);

    if (b == d) {
    	if (b == 0) {
    		if (a && c) {
    			puts("0");
			} else if (a || c){
				puts("1");
			} else {
				puts("2");
			}
		} else {
			ll ans = C(a + b, b) * C(c + b - 1, b - 1) % MOD;
			ans = (ans + C(a + b - 1, b - 1) * C(c + b, b)) % MOD;
			cout << ans << endl;
		}
	} else if(abs(b - d) == 1){
		ll num = (b + d + 1) / 2;
		ll ans = C(a + num - 1, num - 1) * C(c + num - 1, num - 1) % MOD;
		cout << ans << endl;
	} else {
		puts("0");
	}


	return 0;
}
```
