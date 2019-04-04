[题目链接](https://www.hackerrank.com/contests/101hack46/challenges/lena-sort)
【构造】

##题目大意
$q$ 次询问，参数为 $len_i, c_i$
表示要构造的数列长度为$len_i$, 且快速排序比较次数为$c_i$

##思路
每次比较完，相当于将数列分成两部分。
这样相当于一棵二叉树。树的父亲节点就是这次比较的节点。
其左子节点是之后的第一个比其小的数，右子节点是第一个比其大的数。
同时发现其比较次数是该节点的了节点数目。
也就是说每个节点占的权重是其深度，比如深度最小的节点是$n－1$，也就是说是$n-1$个节点的父节点。
所以就是每个节点的深度加起来。
所以我们可以构造一棵树的每层的节点数，据此再来构造这棵树。
我们贪心的思考，考虑到如果某层以下节点数不为0，那么该层节点数也不能为0。那么权值最大的话，每一层就只有一个节点。这样转化为当前权值最大，且我们有其对应的各层的节点数。要做的就是将节点位置调整，从而让当前权值变成目标权值。
这样从后考虑，将深度最大的节点移到合适的位置。比如，比如移到当前所能移动的最小深度（因为是二叉树，每层有节点数目限制）或者移动到的位置正好将权值变为目标权值。
现在有了每层的节点数，也就有了$BFS$序，然后我们将$BFS$序转化为$DFS$序就好。
而且，前序是原数列，中序是排好后的数列。


##代码实现
```cpp
#include <cstdio>
#include <vector>
#include <cstring>
#include <iostream>
#include <algorithm>

#define C(x) cout<<#x<<'='<<x<<endl;
using namespace std;
typedef long long ll;
const int N = 1e5 + 100;
int levels[N];
vector<int> tmp[N], g[N];

int len, c;
int stamp;
int color[N];
void draw(int v) {
	if ( g[v].size() >= 1 ) {
		draw(g[v][0]);
	}
	color[v] = stamp; stamp ++;
	if ( g[v].size() >= 2 ) {
		draw(g[v][1]);
	}
}

void print(int v) {
	printf("%d%c", color[v], stamp == len ? '\n': ' ');
	stamp ++;
	for (int i = 0; i < g[v].size(); i++) print(g[v][i]);
}

int main(void) {
	freopen("in.txt", "r", stdin);

	int q;
	scanf("%d", &q);
	while ( q -- ) {
		scanf("%d%d", &len, &c);

		ll cur = 0;
		for (int i = 0; i < len; i++) {
			levels[i] = 1;
			cur += i;
		}

		int done = 1;
		for (int i = len - 1; i > 0 && cur > c; i--) {
			ll maxr = i - done;
			int r = min(maxr, cur - c);
			levels[i] = 0;
			cur -= r;
			levels[i - r] ++;
			if ( levels[done] == 2 * levels[done - 1] ) done ++;
		}

		if ( cur != c ) {
			puts("-1"); continue;
		}

		stamp = 0;
		for (int i = 0; i < len; i++) {
			for (int j = 0; j < levels[i]; j++) {
				tmp[i].push_back(stamp); stamp ++;
			}
		}

		for (int i = 1; i < len; i++) {
			for (int j = 0; j < levels[i]; j++) {
				g[tmp[i - 1][j / 2]].push_back(tmp[i][j]);
			}
		}

		stamp = 1;
		draw(0);
		stamp = 1;
		print(0);

		for (int i = 0; i < len; i++) {
			tmp[i].clear();
			g[i].clear();
		}
	}


	return 0;
}
```
