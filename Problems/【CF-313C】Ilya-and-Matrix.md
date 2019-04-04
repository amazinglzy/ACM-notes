[题目链接](http://codeforces.com/contest/313/problem/C)

【构造】


## 题目大意
给 $4^n$ 个数，将其放入$2^n \times 2^n$的矩阵中。让其beauty值最大。
先在矩阵中找一个最大的数$m$。
如果矩阵大小是$1$，那么该矩阵的beauty是$m$。
如果不是，那么将矩阵等分成$4$个部分，分别求出$4$个子矩阵的beauty值并加起来。

## 思路
将要相加的数整体考虑。
先加一个数，是矩阵的最大的数。
其次加四个数，是四个子矩阵的最大的数。
然后十六个数等等。
这样贪心的考虑，加的数都是最大的数，然后进行构造，这种情况是可行的。
所以每次加上最大的前$4^k(0 \leq k \leq n)$个数就好。

## 代码实现
```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;
typedef long long ll;

const int N = 2e6 + 100;
ll a[N];
int n;
int der;

bool cmp(ll x, ll y) {
	return x > y;
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	int tmp;
	scanf("%d", &n);
	for (int i = 1; i <= n; i++) {
		scanf("%d", &tmp);
		a[i] = tmp;
	}

	sort(a + 1, a + n + 1, cmp);
	for (int i = 1; i <= n; i++) {
		a[i] += a[i - 1];
	}

	ll ans = 0;
	while (n) {
		ans += a[n];
		n /= 4;
	}

	cout << ans << endl;

	return 0;
}
```
