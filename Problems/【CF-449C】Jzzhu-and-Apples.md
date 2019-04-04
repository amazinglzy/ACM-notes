[题目链接](http://codeforces.com/problemset/problem/449/C)
【构造】

##题目大意
n个数分组，每组2个数，其中gcd是大于1的数，求最多可以分多少组？

##思路
先将 $n/2$ 内的素数求出来，将3开始找，找 $n$ 范围内的 $3$ 的倍数是多少，如果是偶数，那么可直得到部分答案。如果是奇数，那么将2*3留下。
同理依次找。每找到一个数需要标记一下，标记后的数不能再取。
最后找 $2$ 的倍数。

##分析
一个数可以有两个素数来作为其的约数。
所以这个数用于第一个素数和第二个素数是否有区别是这个贪心算法的关键。
想像一种情况，$[a, 2 * a, \dots, k, \dots]$ 和 $[b, 2 * b, \dots, k, \dots]$
设素数$a$的倍数个数是奇数，素数$b$的倍数个数是奇数，这种情况不管$k$放在那个位置，都会有一个$2$的倍数出现，而产生的答案对数是相同的。
如果一个是奇数，一个是偶数，k放在奇数那一列中时，会产生两个$2$的倍数，如果放在偶数那一列中，则不会产生2的倍数，但是产生的答案对数是一样的。
如果两个都是偶数，情况是一样的，都会产生一个$2$的倍数。

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <vector>

using namespace std;
typedef pair<int, int> P;

const int N = 1e5 + 100;

int b[N], primes[N], tot;
int nums[N];
int used[N];

int n;

vector<P> ans;
vector<int> tmp;
vector<int> mul;

int main(void) {
//	freopen("in.txt", "r", stdin);

	scanf("%d", &n);

	tot = 0;

	for (int i = 2; i <= n / 2; i++) {
		if ( !b[i] ) {
			tmp.clear();
			for (int j = 2 * i; j <= n; j += i) {
				b[j] = 1;
			}
			primes[tot ++] = i;

			if (i == 2) continue;
			else {
				tmp.push_back(i);
				for (int j = 2 * i; j <= n; j += i) {
					if ( !used[j] ) {
						used[j] = true;
						tmp.push_back(j);
					}
				}
			}

			if ( (tmp.size() & 1) && tmp.size() != 1 ) {
				ans.push_back(P(tmp[0], tmp[2]));
				for (int j = 4; j < tmp.size(); j += 2) {
					ans.push_back(P(tmp[j], tmp[j - 1]));
				}
				mul.push_back(tmp[1]);
			} else {
				for (int j = 1; j < tmp.size(); j += 2) {
					ans.push_back(P(tmp[j], tmp[j - 1]));
				}
			}
		}
	}

	/*
	for (int i = 0; i < tot; i++) {
		printf("%d ", primes[i]);
	}
	*/
	for (int i = 2; i <= n; i += 2) {
		if ( !used[i] ) mul.push_back(i);
	}

	for (int i = 1; i < mul.size(); i += 2) {
		ans.push_back(P(mul[i], mul[i - 1]));
	}

	printf("%d\n", (int)ans.size());
	for (int i = 0; i < ans.size(); i++) {
		printf("%d %d\n", ans[i].first, ans[i].second);
	}

	return 0;
}

```
