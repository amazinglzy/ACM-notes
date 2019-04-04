[题目链接](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemCode=3765)
# 题目大意
$n$ 个灯，每个灯一个值；
$q$ 个查询；

查询内容如下：

* $Q \ L \ R$ 查询 $[L, R]$ 区间内亮的灯的值的$GCD$；
* $I \ i$ 在$ith$灯后加一个灯；
* $D \ i$ 删除$ith$灯；
* $R \ i$ 转换$ith$灯的状态；
* $M \ i \ x$ 修改$ith$灯的值；

**数据范围：**
$0 \leq n \leq 2e5, 0 \leq q \leq 1e5$
$1 \leq a_i \leq 1e9$


# 代码实现
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e6 + 100;

int num[N], status[N];

int gcd(int a, int b) {
	if (a == -1) return b;
	if (b == -1) return a;
	int c;
	while (b) {
		c = a % b;
		a = b;
		b = c;
	}
	return a;
}

struct splay_t {
	int ch[N][2];
	int pre[N];
	int sz[N];
	int root, tot;

	int val[N];
	int gcd_val[N][2];
	int st[N];

	int new_node(int fa, int v, int state) {
		ch[tot][0] = ch[tot][1] = 0;
		pre[tot] = fa;
		sz[tot] = 1;

		val[tot] = v;
		gcd_val[tot][state] = v;
		gcd_val[tot][state ^ 1] = -1;
		st[tot] = state;
		return tot ++;
	}

	void push_up(int x) {
		sz[x] = sz[ ch[x][0] ] + sz[ ch[x][1] ] + 1;
		gcd_val[x][0] = gcd( gcd_val[ ch[x][0] ][0], gcd_val[ ch[x][1] ][0] );
		gcd_val[x][1] = gcd( gcd_val[ ch[x][0] ][1], gcd_val[ ch[x][1] ][1] );
		gcd_val[x][st[x]] = gcd( gcd_val[ x ][ st[x] ], val[x] );
	}

	void build(int &k, int l, int r, int fa) {
		if (l > r) {
			gcd_val[k][0] = gcd_val[k][1] = -1;
			sz[k] = 0;
			return;
		}
		int mid = (l + r) / 2;
		k = new_node(fa, num[mid], status[mid]);
		build(ch[k][0], l, mid - 1, k);
		build(ch[k][1], mid + 1, r, k);
		push_up(k);
	}

	void init(int n) {
		for (int i = 1; i <= n; i++) {
			scanf("%d%d", &num[i], &status[i]);
		}

		num[0] = -1;
		status[0] = 1;
		num[n + 1] = -1;
		status[n + 1] = 1;

		ch[0][0] = ch[0][1] = 0;
		pre[0] = 0;
		sz[0] = 0;
		val[0] = -1;
		gcd_val[0][0] = -1;
		gcd_val[0][1] = -1;

		tot = 1;
		build(root, 0, n + 1, 0);
	}

	void rotate(int x, int k) {
		int y = pre[x];
		ch[y][k ^ 1] = ch[x][k];
		pre[ ch[x][k] ] = y;
		if ( pre[y] ) ch[ pre[y] ][ ch[pre[y]][1] == y ] = x;
		pre[x] = pre[y];
		ch[x][k] = y;
		pre[y] = x;
		push_up(y);
	}

	void splay(int x, int goal) {
		while (pre[x] != goal) {
			int y = pre[x];
			if (pre[y] == goal) {
				rotate(x, ch[y][0] == x);
			} else {
				int k = ch[pre[y]][0] == y;
				if (ch[y][k] == x) {
					rotate(x, k ^ 1);
					rotate(x, k);
				} else {
					rotate(y, k);
					rotate(x, k);
				}				
			}
		}
		push_up(x);
		if (goal == 0) root = x;
	}

	void rotate_to(int k, int goal) {
		int cur = root;
		while (sz[ch[cur][0]] + 1 != k) {
			if (sz[ch[cur][0]] + 1 < k) {
				k -= (sz[ch[cur][0]] + 1);
				cur = ch[cur][1];
			} else {
				cur = ch[cur][0];
			}
		}
		splay(cur, goal);
	}

	int query(int l, int r, int s) {
		rotate_to(l, 0);
		rotate_to(r + 2, root);
		return gcd_val[ ch[ch[root][1]][0] ][s];
	}

	void insert(int i, int val, int st) {
		rotate_to(i + 2, 0);
		if (ch[root][0] == 0) {
			ch[root][0] = new_node(root, val, st);
			push_up(root);
			return;
		}

		int cur = ch[root][0];
		while (ch[cur][1]) cur = ch[cur][1];

		splay(cur, root);

		ch[cur][1] = new_node(cur, val, st);

		push_up(cur);
		push_up(root);
	}

	void del(int i) {
		rotate_to(i + 1, 0);
		if (ch[root][0] == 0) {
			root = ch[root][1];
			pre[root] = 0;
			return;
		}

		int cur = ch[root][0];
		while (ch[cur][1]) cur = ch[cur][1];

		splay(cur, root);

		ch[cur][1] = ch[root][1];
		pre[ch[cur][1]] = cur;

		root = cur;
		pre[root] = 0;
		push_up(root);
	}

	void toggle(int i) {
		rotate_to(i + 1, 0);
		st[root] = st[root] ^ 1;
		push_up(root);
	}

	void change(int i, int x) {
		rotate_to(i + 1, 0);
		val[root] = x;
		push_up(root);
	}

} splay;


int main(void) {

	int n, q;
	while (~scanf("%d%d", &n, &q)) {
		splay.init(n);

		char od[5];
		int l, r, i, s, x;
		while ( q -- ) {
			scanf("%s", od);
			if (od[0] == 'Q') {
				scanf("%d%d%d", &l, &r, &s);
				printf("%d\n", splay.query(l, r, s));
			} else if (od[0] == 'I') {
				scanf("%d%d%d", &i, &x, &s);
				splay.insert(i, x, s);
			} else if (od[0] == 'D') {
				scanf("%d", &i);
				splay.del(i);
			} else if (od[0] == 'R') {
				scanf("%d", &i);
				splay.toggle(i);
			} else {
				scanf("%d%d", &i, &x);
				splay.change(i, x);
			}
		}
	}


	return 0;
}
```
