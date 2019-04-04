[题目链接](http://codeforces.com/contest/671/problem/B)
【二分】

##题目大意
$n$个人有$c_i$个硬币，$k$个操作，每次操作使得硬币最多中的那个人的一枚硬币到最少中的一个人中。
求$k$次之后最多和最少硬币的人的硬币差。

##思路
将操作分为两部分。
第一部分是富人每天减少一块硬币。
第二部分是穷人每天增加一块硬币。

这样，分别二分最富的人硬币y和最贫的人硬币x，对其进行分类讨论。
注意如果$x = y$，那么需要看硬币总和是否余$n$。

##代码实现
```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>

#define OUT(x) cout<<#x<<'='<<x<<endl;
using namespace std;
typedef long long LL;
const int N = 5e5 + 100;
const int INF = 0x3f3f3f3f;
int a[N], n, k; bool poor(int x) {
    LL days = 0;
    for (int i = 0; i < n; i++) {
        if ( x> a[i]) {
			days += x - a[i];
		} else {
			break;
		}
	}
	return days > k;
}

bool rich(int x) {
	LL days = 0;
	for (int i = n - 1; i >= 0; i--) {
		if ( a[i] > x ) {
			days += a[i] - x;
		} else {
			break;
		}
	}
	return days <= k; }
int main(void) {
    // freopen("in.txt", "r", stdin);
	LL sum = 0;
	int mina = INF, maxa = 0;
	scanf("%d%d", &n, &k);
	for (int i = 0; i < n; i++) {
	    scanf("%d", a + i);
	    sum += a[i];
	    mina = min(mina, a[i]);
	    maxa = max(maxa, a[i]);
	}
    // OUT(sum);
    sort(a, a + n);
    /* for (int i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    printf("\n"); */
    int x, y;
    LL left = 0, right = sum, mid;
    while ( right - left> 1 ) {
	    mid = (left + right) / 2;
	    if ( poor(mid) ) right = mid;
	    else left = mid;
    }

	x = left;

	left = 0; right = sum;
	while ( right - left > 1 ) {
		mid = (left + right) / 2;
		if ( rich(mid) ) right = mid;
		else left = mid;
	}

	y = right;
//	printf("%d %d\n", x, y);


	if ( x >= y) {
		printf("%d\n", sum % n ? 1 : 0);
	} else {
		printf("%d\n", y - x);
	}


	return 0;
}
```
