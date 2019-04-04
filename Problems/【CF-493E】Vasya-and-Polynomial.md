[题目链接](http://codeforces.com/problemset/problemz493/E)
【构造】

##题目大意
定义$P(x) = a_0 + a_1 x + a_2 x^2 + a_3 x^3 + \dots + a_n x^n $
已知t、a、b，求使得$P(t) = a, P(a) = b$的$[a_0, a_1, \dots, a_n]$的种类有多少？

##思路
已知$a_i$非负，$t, a, b$是正整数。
如果t为1，那么$a = a_0 + a_1 + \dots + a_n$，这样$a_i$最大为a。
如果存在一个$a_i$是$a$，那么有且仅有一个是$a$，所以是求$a * a^k = b$，找k就好。这个时候，如果$a$是$1$，那么答案是 $INF $。
如果t不为1, 那么只有$a_0$是可能是$a$，那么如果$a = b$也算是一种情况，否则$a_i$都小于$a$， 那么相当于$b$用$a$进制表示，当然只有一种情况。需要算下系数，然后验证下。
如果$a$是$1$，那么$a_i$的一个是$1$，同时$b$也是$1$，这种情况当$t$是$1$的时候已经讨论过，没有别的情况了。

##代码实现
T_T版本
```cpp
#include <cstdio>
#include <iostream>

using namespace std;
typedef long long LL;

LL t, a, b;

int main(void) {
//	freopen("in.txt", "r", stdin);

	cin >> t >> a >> b;

	int ans = 0;
	if ( t == 1 ) {
		if ( a == b && a == 1 ) {
			puts("inf");
			return 0;
		} else if (a != 1){
			LL tmp = b, times = 0;
			while ( tmp % a == 0 ) tmp /= a, times ++;
			if ( tmp == 1 && times != 1 && times != 0) ans ++;
		}
	}

	if ( a != 1 ) {
		LL tmp = b;
		LL check = 0;
		LL tmpt = 1;
		while ( tmp ) {
			if ( tmp % a ) {
				check += tmpt * (tmp % a);
			}
			tmpt *= t;
			tmp /= a;
		}

		if ( check == a ) ans ++;

	}
	if ( a == b ) ans ++;
	printf("%d\n", ans);

	return 0;
}
```
