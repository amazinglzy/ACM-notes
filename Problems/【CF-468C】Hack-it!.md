[题目链接](http://codeforces.com/problemset/problem/468/C)
【尺取法】【二分】

##题目大意
定义 $f(i)$ 为十进制为 $i$ 的各个位数和。
求$\sum_{i = l}^r f(i) = 0 (mod \ a)$的 $l, r$

##思路
设$s(i) = \sum_{i = 1}^r f(i)$
题目可以转化为求$s(r) - s(l-1) == 0 (mod \ a)$
所以目标是找一对$l, r$使得等式成立。
先二分找一个$r$使得$s(r) > a$
然后尺取就可以了。

（下面的前方是该数位之前的部分的数的大小）
这里求$s(i)$是方法是考虑每一个数位，然后算当它是确定时，可能的数有多少个。
首先，从后往前考虑。每个数位确定后，其变化的可能是该数位前方和后方。
如果这个数位上的数是$5$，这样将$0-9$分为三部分（即使是5，这个位置上还是会出现9的），一部分是$5$以下，这个时候，前方的数只要不超过本来的数就可以，后方都可以，只要位数相同就行。一部分是$5$，当前方数小于其本身的值时，后方任何数都可以，当前方的数为其本身时，后方数不能超过本身。第三部分是其他的，前方是上一部分的前方$-1$, 如果是其本身的值，就超了。后方则都可以，只要位数一样就行。

##代码实现
```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>

#define C(x) cout<<#x<<'='<<x<<endl;
using namespace std;
typedef long long LL;
LL f(LL x) {
    LL ret = 0;
    for (LL len = 1; len <= x; len *= 10) {
        LL dig = (x / len) % 10;
        LL cur = 0; LL m = x / (10 * len);
        LL n = x % len;
        for (LL i = 1; i < dig; i++) cur += i;
        ret += (m + 1) * cur * len;
        ret += dig * (m * len + n + 1);
        for (LL i = dig + 1; i <= 9; i++) ret += m * len * i;
    } return ret;
}

LL g(LL x) {
    LL ret = 0;
    while ( x ) {
        ret += x % 10; x /= 10;
    }
    return ret;
}
LL a;
int main(void) {
// freopen("in.txt", "r", stdin);
    cin>> a;
	LL l = 1, r = 1e17;
	while ( r - l > 1 ) {
		LL mid = (l + r) / 2;
		if ( f(mid) < a ) l = mid;
		else r = mid;
	}
	l = 1;
	LL sum = f(r) - f(l - 1);
	while ( 1 ) {
		if ( sum < a ) sum += g(++r);
		else if ( sum > a ) sum -= g(l++);
		else break;
	}
	cout << l << " " << r << endl;


	return 0;
}
```
