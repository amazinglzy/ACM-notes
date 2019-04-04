[题目链接](http://codeforces.com/problemset/problem/15/C)

##题目大意
* 取石子
* 堆数为$nm$
* 每堆数分别为$x_i + 1, x_i + 2, \cdots, x_i + m_i - 1 \text (0 \leq i \lt n)$
* $\text(1 \leq n \leq 10^5, 1 \leq x_i, m_i \leq 10^{16} )$

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
typedef unsigned int ui;
const int N = 1e5 + 100;
const ll INF = 1e16 + 100;

int a[N];

ll f(ll x) {
	ll ret = 0;
	for (ll i = 1; i < INF; i <<= 1) { ll t = i * 2; ll num = x / t * i; if ( x % t>= i ) num += x % t - i + 1;
		if (num & 1) ret |= i;
	}
	return ret;
}

int main() {
//	freopen("in.txt", "r", stdin);

	ll n, m, x;

	ll ans = 0;
	scanf("%I64d", &n);
	for (int i = 0; i < n; i++) {
		scanf("%I64d%I64d", &x, &m);
		ans ^= f(x + m - 1);
		ans ^= f(x - 1);
	}
	puts(ans ? "tolik" : "bolik");

    return 0;
}
```
