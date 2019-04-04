[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=4453)

# 题目大意
$n$ 个元素排成一个圈；
每个元素有一个值；
给定一个当前位置的指针，两个参数 $k1$ 和 $k2$；

已知操作如下：

* $add \ x$ 从当前指向的元素开始，顺时针数 $k_2$ 个元素，并将这些元素的值加上 $x$；
* $reverse$ 从当前指向的元素开始，顺时针数 $k_1$ 个元素，这些元素翻转；
* $insert \ x$ 在当前指向的元素顺时针方向上的下一个位置添加一个元素，且值为　$x$；
* $delete \ x$ 删除当前指向的元素，并将该指针指向未删除该元素时的顺时针方向的下一个元素；
* $move \ x$ 如果 $x = 1$ 该指针逆时针移动一个元素，否则顺时针移动一个元素；
* $query \ $ 输出当前指向的元素的值；

**数据范围：**
$2 \leq k_1 \lt k_2 \leq N \leq 10^5, M \leq 10^5$
$-10^4 \leq a_i \leq 10^4$

```cpp
#include <bits/stdc++.h>

#define MIN(x, y) ((x)<(y)?(x):(y)) #define MAX(x, y) ((x)>(y)?(x):(y))
#define MEM(x, y) memset(x, y, sizeof(x))

using namespace std;
typedef long long LL;
typedef pair<int, int> P;

const int N = 3e5 + 100;
int a[N], n, m, k1, k2;

struct splay_t {
	int ch[N][2];
	int pre[N];
	int sz[N];
	int root, total;

	int val[N];
	int add[N];
	int rev[N];
	int num;

	int new_node(int fa, int v) {
		ch[total][0] = ch[total][1] = 0;
		pre[total] = fa;
		sz[total] = 1;

		val[total] = v;
		add[total] = 0;
		rev[total] = 0;
		return total ++;
	}

	void push_up(int k) {
		sz[k] = sz[ch[k][0]] + sz[ch[k][1]] + 1;
	}

	void push_down(int k) {
		if (add[k]) {
			if (ch[k][0]) {
				add[ch[k][0]] += add[k];
				val[ch[k][0]] += add[k];
			}
			if (ch[k][1]) {
				add[ch[k][1]] += add[k];
				val[ch[k][1]] += add[k];
			}
			add[k] = 0;
		}

		if (rev[k]) {
			swap(ch[k][0], ch[k][1]);
			if (ch[k][0]) {
				rev[ch[k][0]] ^= 1;
			}
			if (ch[k][1]) {
				rev[ch[k][1]] ^= 1;
			}
			rev[k] ^= 1;
		}
	}

	int build(int l, int r, int fa) {
		if (l > r) return 0;
		int mid = (l + r) / 2;
		int ret = new_node(fa, a[mid]);
		ch[ret][0] = build(l, mid - 1, ret);
		ch[ret][1] = build(mid + 1, r, ret);
		push_up(ret);
		return ret;
	}

	void init(int n) {
		num = n;
		for (int i = 1; i <= n; i++) {
			scanf("%d", &a[i]);
		}
		a[0] = 0;
		a[n + 1] = 0;

		ch[0][0] = ch[0][1] = 0;
		pre[0] = 0;
		sz[0] = 0;
		total = 1;
		val[0] = 0;
		add[0] = 0;
		rev[0] = 0;

		root = build(0, n + 1, 0);
	}

	void rotate(int x, int k) {
		int y = pre[x];
		ch[y][k ^ 1] = ch[x][k];
		pre[ch[x][k]] = y;
		if (pre[y]) ch[pre[y]][ ch[pre[y]][1] == y ] = x;
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
		push_down(cur);
		while (sz[ch[cur][0]] + 1 != k) {
			if (sz[ch[cur][0]] + 1 < k) {
				k -= sz[ch[cur][0]] + 1;
				cur = ch[cur][1];
			} else {
				cur = ch[cur][0];
			}
			push_down(cur);
		}
		splay(cur, goal);
	}

	void seg_add(int x) {
		rotate_to(1, 0);
		rotate_to(k2 + 2, root);
		int k = ch[ch[root][1]][0];
		add[k] += x;
		val[k] += x;
	}

	void reverse() {
		rotate_to(1, 0);
		rotate_to(k1 + 2, root);
		int k = ch[ch[root][1]][0];
		rev[k] ^= 1;
	}

	void insert(int x) {
		num ++;
		rotate_to(3, 0);
		rotate_to(2, root);
		int k = ch[root][0];
		ch[k][1] = new_node(k, x);
		pre[ch[k][1]] = k;
		push_up(k);
		push_up(root);
	}

	void del() {
		num --;
		rotate_to(3, 0);
		rotate_to(1, root);
		int k = ch[root][0];
		ch[k][1] = 0;
		push_up(k);
		push_up(root);
	}

	void move(int x) {
		if (x == 1) {
			rotate_to(num + 1, 0);
			int v = val[root];
			rotate_to(num, root);
			int k = ch[root][0];
			ch[k][1] = ch[root][1];
			pre[ch[root][1]] = k;
			root = k;
			pre[k] = 0;
			push_up(root);

			rotate_to(2, 0);
			rotate_to(1, root);
			k = ch[root][0];
			ch[k][1] = new_node(k, v);
			pre[ch[k][1]] = k;
			push_up(k);
			push_up(root);
		} else {
			rotate_to(2, 0);
			int v = val[root];
			del();
			rotate_to(num + 1, 0);
			rotate_to(num + 2, root);
			int k = ch[root][1];
			ch[k][0] = new_node(k, v);
			pre[ch[k][0]] = k;
			push_up(k);
			push_up(root);
			num ++;
		}
	}

	int query() {
		rotate_to(2, 0);
		return val[root];
	}

	void tran(int k) {
		if (!k) return;
		push_down(k);
		tran(ch[k][0]);
		//printf("lson: %d, rson: %d, sz: %d\n", ch[k][0], ch[k][1], sz[k]);
		printf("%d ", val[k]);
		tran(ch[k][1]);
	}

} splay;

int main(void) {
	int cnt = 1;

	while (scanf("%d%d%d%d", &n, &m, &k1, &k2), n || m || k1 || k2) {
		printf("Case #%d:\n", cnt ++);

		splay.init(n);
//		splay.tran(splay.root); puts("");

		char cmd[10]; int x;
		for (int i = 0; i < m; i++) {
			scanf("%s", cmd);
//			puts(cmd);
			//splay.tran(splay.root); puts("");
			if (strcmp(cmd, "add") == 0) {
				scanf("%d", &x);
				splay.seg_add(x);

//				splay.tran(splay.root); puts("");
			} else if (strcmp(cmd, "reverse") == 0) {
				splay.reverse();

//				splay.tran(splay.root); puts("");
			} else if (strcmp(cmd, "insert") == 0) {
				scanf("%d", &x);
				splay.insert(x);

//				splay.tran(splay.root); puts("");
			} else if (strcmp(cmd, "delete") == 0) {
				splay.del();

//				splay.tran(splay.root); puts("");
			} else if (strcmp(cmd, "move") == 0) {
				scanf("%d", &x);
				splay.move(x);

//				splay.tran(splay.root); puts("");
			} else {
				printf("%d\n", splay.query());
			}
		}
	}

	return 0;
}


```
