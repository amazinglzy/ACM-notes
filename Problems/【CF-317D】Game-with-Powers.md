[题目链接](http://codeforces.com/contest/317/problem/D)
【Nim博弈】

##题目大意
$1 - n$的数，一个人选一个数之后，其平方、立方等等都不能再选。
现两个人先后轮流选数，谁先选完谁赢。

##思路
$1-n$的数中，每次选一个数，相当于当这个数及这个数的$k$次方的数全部拿走。
有两种情况，一是这个数是另一个数的$k$次方，另一种是这个数不是。
考虑第二种情况，一个不是另一个数的$k$次方的数可以将这$n$个数划分成两个集合，一个集合里的数是这个数的幂次，另一个是其它的数。
再考虑第一种情况，如果这个数是另一个数的$k$次方，那么拿走的数一定在这个另一数划分的集合里。
并且集合与集合是不相交的。
所以这相当于在$k$堆石子里依次取子。但取法还是不一样的。需要算出每个集合的$sg$值。
如果将集合里的数写成幂的形式，就可以用指数代替这个数。所以每个集合的$sg$值只与集合大小有关。考虑集合如最大的情况，集合中最小的数是$2$，$2^k = n$，所以可以暴力算出$sg$值，打表使用，直接异或就好了。

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
typedef long long ll;

const int N = 30;
const int M = 1e6 + 100;

/*
map<int, int> dp;
int _sg(int mask) {
	if (dp.count(mask)) return dp[mask];
	set<int> mex;
	for (int i = 0; i < N; i++) {
		if (!((mask >> i) & 1) ) continue;
		int tp = mask;
		for (int j = i; j < N; j += i + 1) {
			if ((mask >> j) & 1) tp ^= 1 << j;
		}
		mex.insert(_sg(tp));
	}
	int ret = 0;
	for (; mex.count(ret); ret ++);
	return dp[mask] = ret;
}
*/

int sg[] = {0, 1, 2, 1, 4, 3, 2, 1, 5, 6, 2, 1, 8, 7, 5, 9, 8, 7, 3, 4, 7, 4, 2, 1, 10, 9, 3, 6, 11, 12};
int n, sqn;
int f[N];
bool vis[M];

int main(void) {
//    freopen("in.txt", "r", stdin);

    /*
    dp[0] = 0;
	for (int i = 1; i < N; i++) {
    	sg[i] = _sg((1 << i) - 1);
    	printf("%d\n", sg[i]);
	}
	*/

   	scanf("%d", &n);
   	for (sqn = 1; sqn * sqn <= n; sqn ++);
   	-- sqn;
   	f[1] = n - sqn + 1;

	int mx = 1;
	for (int i = 2; i <= sqn; i++) {
		if (vis[i]) continue;
		int cnt = 0, cur = 1, lim = n / i;
		for (; cur <= lim; ++cnt) {
			cur *= i;
			if (cur <= sqn) vis[cur] = 1;
			else --f[1];
		}
		++f[cnt];
		if ( mx < cnt) mx = cnt;
	}

	int ans = 0;
	for (int i = 1; i <= mx; i++) {
		if (f[i] & 1) ans ^= sg[i];
	}
	puts(ans ? "Vasya" : "Petya");

	return 0;
}
```
