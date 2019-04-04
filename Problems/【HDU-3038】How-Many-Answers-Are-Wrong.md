[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=3038)

【并查集】【模拟】

##题目大意
有n个数，给m个answers，每个answer是区间[l,r]的数的和，问有多少个answer是错的。
(1 <= N <= 200000, 1 <= M <= 40000) ##思路 模拟就好，用并查集维护。 并查集是一棵树。 现把每个数看成一个节点，节点与节点的边的权值就是两个数形成的区间的和。 通过路径压缩之后，节点的边权值就该节点到其父节点的权值。 现给一个区间，如果这两个数在同一棵树内，可以看两个节点的边权值之差与给定的和是否相等来判断否是是错的。 ##扩展 如果是找最少的answers使得其不再冲突，怎么处理？ m个answer中，有一些是与另外的冲突的。 首先冲突的来源是什么？比如[a, b], [b, c], [a, c]三个区间，已知[a, b]的和为x，[a, c]和为y，这样[b, c]是y - x,一旦有一个answer是[b, c] 是 t(t != y - x)，那么便产生了冲突。 要怎么处理？首先这四个关系只用去除一个就暂时不会冲突。如果接下来[a, d], [d, b]使得其与[a, b]冲突，只用除掉[a, b]区间就好了。 ##代码 ```cpp #include <cstdio>

using namespace std;

const int N = 2e5 + 100;
int p[N], d[N], n, m;

int find(int x) {
	if ( x == p[x] ) return x;
	int root = find(p[x]);
	d[x] += d[p[x]];
	return p[x] = root;
}

int main(void) {
	freopen("in.txt", "r", stdin);


	while(~scanf("%d%d", &n, &m)) {
		int ans = 0;
		for (int i = 0; i <= n; i++) {
			p[i] = i; d[i] = 0;
		}

		for (int i = 0; i < m; i++) {
			int l, r, sum;
			scanf("%d%d%d", &l, &r, &sum);
			int p1 = find(l - 1), p2 = find(r);
			if ( p1 == p2 ) {
				if (sum != d[r] - d[l - 1]) ans ++;
			} else {
				p[p2] = p1;
				d[p2] = sum + d[l - 1] - d[r];
			}
		}
		printf("%d\n", ans);
	}


	return 0;
}
```
