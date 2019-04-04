[题目链接](https://open.kattis.com/problems/cupid)
【莫队算法】

##题目大意
两个数列$[a_1, a_2, \dots, a_n], [b_1, b_2, \dots, b_n]$
m个询问，每次询问区间$[l, r]$中有第一个数列和第二个数列有多少对数相同。

##思路
**莫队算法**
转移的话用两个数组存当前区间的各个数的个数。

##代码实现
```cpp
#include <cstdio>
#include <cmath>
#include <algorithm>

using namespace std;

const int N = 5e4 + 100;
const int K = 1e6 + 100;

int n, m, k;
int a[N], b[N];

struct Query { int l, r, id, ans; } q[N];
int unit;
bool cmp_block(Query a, Query b) {
	return a.l / unit == b.l / unit ? a.r < b.r : a.l / unit < b.l / unit;
}
bool cmp_id(Query a, Query b) {
	return a.id < b.id;
}

int man[K], wem[K];
void work() {
	int l = 0, r = 0, ans = 0;
	man[a[l]] ++; wem[b[r]] ++;
	if ( a[l] == b[l] ) ans ++;

	for (int i = 0; i < m; i++) {
		while ( r < q[i].r ) {
			r ++;
			man[a[r]] ++;
			if ( wem[a[r]] >= man[a[r]] ) ans ++;
			wem[b[r]] ++;
			if ( man[b[r]] >= wem[b[r]] ) ans ++;
		}

		while ( l > q[i].l ) {
			l --;
			man[a[l]] ++;
			if ( wem[a[l]] >= man[a[l]] ) ans ++;
			wem[b[l]] ++;
			if ( man[b[l]] >= wem[b[l]] ) ans ++;
		}

		while ( r > q[i].r ) {
			if ( wem[a[r]] >= man[a[r]] ) ans --;
			man[a[r]] --;
			if ( man[b[r]] >= wem[b[r]] ) ans --;
			wem[b[r]] --;
			r --;
		}

		while ( l < q[i].l ) {
			if ( wem[a[l]] >= man[a[l]] ) ans --;
			man[a[l]] --;
			if ( man[b[l]] >= wem[b[l]] ) ans --;
			wem[b[l]] --;
			l ++;
		}


		q[i].ans = ans;
	}
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	scanf("%d%d%d", &n, &m, &k);
	for (int i = 0; i < n; i++) {
		scanf("%d", a + i);
	}

	for (int i = 0; i < n; i++) {
		scanf("%d", b + i);
	}

	unit = (int)sqrt(n);

	for (int i = 0; i < m; i++) {
		q[i].id = i;
		scanf("%d%d", &q[i].l, &q[i].r);
	}

	sort(q, q + m, cmp_block);
	work();
	sort(q, q + m, cmp_id);

	for (int i = 0; i < m; i++) {
		printf("%d\n", q[i].ans);
	}


	return 0;
}
```
