[题目链接](http://codeforces.com/contest/300/problem/E)
【二分】

##题目大意
$$ n! \ \% \ \prod_{i = 0}^{n - 1} a_i ! = 0  $$
给定$a_i$，求$n$。

##思路
二分答案。
考虑因子来判断$n$是否合法。 $n!$ 的因子$p$个数是$\lfloor \frac n p \rfloor + \lfloor \frac n p^2 \rfloor + \dots$
这样就可以预处理出范围内的素数，然后依次判断。
分母的因子个数可以将$a_i$整体考虑，对于$a_i$，其是$1-a_i$的乘积，所以在$1-a_i$对应的因子位置上加 $1$，这样我们得到了分母的所有的因子，只用将是合数的因子转化为素数就好了。所以我们从后往前扫，将合数分解就好。是素数就记录下个数就好。
在记录仪$a_i$因子时，相当于在$1-a_i$这个区间加$1$，可以用差分的方式修改，最后再求前缀和，再在这个基础上修改就好。


##代码实现
```cpp
#include <cstdio>
#include <vector>
#include <cstring>
#include <iostream>

using namespace std;

typedef long long ll;
typedef pair<int, ll> P;

const int N = 1e7 + 100;
const ll INF = 1e13;
bool mark[N];
int primes[N], tot;

ll num[N]; int n;


int m;
int a[N];

vector<P> st;

bool ok(ll x) {

	for (int i = 0; i < st.size(); i++) {
		ll cp = x;
		ll ruler = st[i].second;
		int cu = st[i].first;
		while ( cp ) {
			ruler -= cp / cu;
			cp /= cu;
		}
		if (ruler > 0) return false;
	}
	return true;

	return false;
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	tot = 0;
	memset(mark, 0, sizeof(mark));
	for (int i = 2; i * i <= N; i++) {
	    if (!mark[i]) {
	        for (int j = i * i; j < N; j += i) mark[j] = true;
	    }
	}
	for (int i = 2; i < N; i++)
	    if (!mark[i])
	        primes[tot ++] = i;
    n = 0;
    scanf("%d", &m);
    for (int i = 0; i < m; i++) {
        scanf("%d", &a[i]);
        n = max(n, a[i] + 1);
    }
    for (int i = 0; i < m; i++) {
        num[1] += 1;
        num[a[i] + 1] -= 1;
    }
    for (int i = 1; i <= n; i++) {
        num[i] += num[i - 1];
    }
    for (int i = n; i> 1; i--) {
		if ( ! mark[i] && num[i]) {
			st.push_back(P(i, num[i]));
		} else {
			int tmp = num[i];
			for (int j = 0; j < tot && primes[j] < i; j++) {
				if (i % primes[j] == 0) {
					num[primes[j]] += tmp;

					num[i/primes[j]] += tmp;				
					num[i] = 0;

					break;
				}
	    	}
		}
	}

	/*
	printf("%d\n", (int)st.size());
	for (int i = 0; i < st.size(); i++) {
		printf("%d %d\n", st[i].first, st[i].second);
	}
	*/


	ll left = 0, right=  INF;
	while (right - left > 1) {
		ll mid = (left + right) / 2;
		if (ok(mid)) right = mid;
		else left = mid;
	}
	cout << right << endl;

	return 0;
}

```
