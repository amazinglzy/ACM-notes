[题目链接](http://codeforces.com/contest/348/problem/C)
【模拟】【懒标记】

##题目大意
给一个数组a, m个集合，每个集合是数组中的一些数（非连续）。
两种操作。
第一种是给某个集合的所有元素加x。
第二种是求某个集合的和。

##思路
一个数可能在多个集合里，一个集合也有多个数。
对集合的修改，会对多个集合造成影响。
可以对要修改的集合里的所有元素都遍历一遍，对元素进行修改。这样修改的复杂度是$O(n)$，查询的复杂度是$O(n)$。如果集合的元素多，那么可能会超时。
也可以先处理出各个集合的交集的元素的个数。这样，可以用一个标记来记录修改，当查询的时候，计算查询的集合与各个集合的交集的元素的个数。这样只用维护各个集合的和就好。如果集合的数目过多，效率也不会太高。

题目有一个条件是集合的元素总的个数不会超过 $1e5$个。
这样，可以将这两种情况结合起来。将集合划分成大集合和小集合，大集合的个数不会太多，小集合的话内部的元素也不会太多，这样，就可以将这个题过了。


##代码实现
```cpp
#include <cstdio>
#include <cmath>
#include <set>
#include <vector>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;
typedef long long LL;

const int N = 1e5 + 100;
const int W = 400;

vector<int> big;
set<int> s[N];
set<int> belong[N];
int isbig[N];

int n, m, q;
LL a[N];

bool vis[N];
int intersection[W][N];

LL sum[N], tag[N];
LL tagsum(int k) {
	LL ret = 0;
	for (int i = 0; i < big.size(); i++) {
		if ( intersection[i][k] ) ret += tag[i] * intersection[i][k];
	}
	return ret;
}

int main(void) {
//	freopen("in.txt", "r", stdin);
	memset(isbig, -1, sizeof(isbig));

	scanf("%d%d%d", &n, &m, &q);
	for (int i = 0; i < n; i++) scanf("%I64d", &a[i]);

	int lev = sqrt(n);
	int num, tmp; LL tot;
	for (int i = 0; i < m; i++) {
		scanf("%d", &num);
		tot = 0;
		for (int j = 0; j < num; j++) {
			scanf("%d", &tmp);
			tmp --;

			s[i].insert(tmp);
			belong[tmp].insert(i);

			tot += a[tmp];
		}

		if ( num > lev ) {
			big.push_back(i);
			isbig[i] = big.size() - 1;
			sum[big.size() - 1] = tot;
		}
	}


	for (int i = 0; i < big.size(); i++) {
		memset(vis, 0, sizeof(vis));

		int ci = big[i];
		for (set<int>::iterator it = s[ci].begin(); it != s[ci].end(); it++) {
			vis[*it] = true;
		}

		for (int j = 0; j < m; j++) {
			for (set<int>::iterator it = s[j].begin(); it != s[j].end(); it ++) {
				if ( vis[*it] ) intersection[i][j] ++;
			}
		}
	}

	/*
	for (int i = 0; i < m; i++) {
		printf("%d ", sum[i]);
	}
	printf("\n");
	*/

	/*
	for (int i = 0; i < big.size(); i++) {
		printf("%d\n", big[i]);
		for (int j = 0; j < m; j++) {
			printf("%d ", intersection[i][j]);
		}
		printf("\n");
	}
	*/

	char cmd[8];
	int k, x;
	while ( q -- ) {
		scanf("%s", cmd);
		if ( cmd[0] == '+' ) {
			scanf("%d%d", &k, &x);
			k --;

			if ( isbig[k] != -1 ) {
				tag[isbig[k]] += x;
			} else {
				set<int>::iterator ti, tj;
				for (ti = s[k].begin(); ti != s[k].end(); ti ++) {
					a[*ti] += x;
				}
				for (int i = 0; i < big.size(); i++) {
					if ( intersection[i][k] ) sum[i] += x * intersection[i][k];
				}
			}

		} else {
			scanf("%d", &k);
			k --;

			if ( isbig[k] != -1 ) {
				LL ans = sum[isbig[k]];
				ans += tagsum(k);
				cout << ans << endl;
			} else {
    			LL ans = 0;
    			for (set<int>::iterator it = s[k].begin(); it != s[k].end(); it ++) {
					ans += a[*it];
				}
				ans += tagsum(k);
				cout << ans << endl;
			}
		}
	}

	return 0;
}
```
