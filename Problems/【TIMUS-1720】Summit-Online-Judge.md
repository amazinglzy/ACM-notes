[题目链接](http://acm.timus.ru/problem.aspx?space=1&num=1720)
【构造】

##题目大意
给两个区间$S、T$，求$S$中的任意数相加，和有多少在$T$中？

##思路

最小的区间是可以$[l, r]$，其次是$[2l, 2r]$，等等。
只考虑区间的变化，如果区间的长度不是$1$，那么区间最终会合并到一起。
按照这个规律，就可以算出来了。

##代码实现
```cpp
#include <cstdio>
#include <algorithm>
using namespace std;
typedef long long LL;

LL solve(LL n, LL x, LL y)
{
	if (x == y) return n / x;
	LL a = (x - 1) / (y - x) + 1, b = n / x;
	if(a <= b)
		return a * (a - 1) / 2 * (y - x) + a - 1 + n - a * x + 1;
	return b * (b - 1) / 2 * (y - x) + b - 1 + min(n, b * y) - b * x + 1;
}

int main()
{
	LL x, y, L, R;
	scanf("%lld%lld%lld%lld", &x, &y, &L, &R);
	printf("%lld\n", solve(R, x, y) - solve(L - 1, x, y));
	return 0;
}
```
