[题目链接](http://codeforces.com/problemset/problem/85/D)
【离散化】【线段树】

##题目大意
一个集合，$add \ x$在集合中加入$x$，$del \ x$在集合中减去$x$，$sum $先排序后的集合。

$$sum = \sum_{i \ mod \ 5 \ = \ 3}^{i \leq k } a_i$$

##思路
用一棵线段树维护当前数值在$[l, r]$的数模$5$的同剩余类的和是多少以及这个区间的数是多少。
这样，答案是$root$的模$5$余$2$的剩余类的和。

用$dp[v][i]$表示节点$v$代表的区间的所有的数在次序上模$5$余$i$的和是多少。
至于区间的合并，先考虑一个区间节点的左子，设里面的数的个数是$a$，左区间的值可以直接加到父节点上，右子根据转换下顺序就好。

因为数的范围大，所以需要离散化一下。

##代码实现

```cpp
#include <cstdio>
#include <cstring>
#include <set>
#include <map>
#include <iostream>

using namespace std;
typedef long long ll;

set<int> tmp;
map<int, int> cp;

const int N = 1e5 + 100;

ll sum[4*N][6], num[4*N], m;
int v[N];

void update(int k, int l, int r, int p, int x) {
	if (l == r) {
		num[k] += x;
		for (int i = 0; i < 5; i++) sum[k][i] = 0;
		if (num[k]) sum[k][0] += v[l];
		return;
	}
	if (p <= (l + r) / 2)
	    update(2 * k + 1, l, (l + r) / 2, p, x);
	else
	    update(2 * k + 2, (l + r) / 2 + 1, r, p , x);
	num[k] = num[2 * k + 1] + num[2 * k + 2];
	for (int i = 0; i < 5; i++) {
	    sum[k][i] = sum[2 * k + 1][i] + sum[2 * k + 2][(i + 5 - num[2 * k + 1] % 5) % 5];
	}
}

char cm[N][10];
int arg[N], n;
int main(void) {
    // freopen("in.txt", "r", stdin);
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%s", cm[i]);
        if (strcmp(cm[i], "add") == 0 ) {
            scanf("%d", &arg[i]);
            tmp.insert(arg[i]);
        } else if(strcmp(cm[i], "del") == 0 ){
            scanf("%d", &arg[i]); tmp.insert(arg[i]);
        }
    }
    m = 0;
    for(set<int>::iterator it = tmp.begin(); it != tmp.end(); it ++) {
		cp[*it] = ++m; v[m] = *it;
	}


	for (int i = 0; i < n; i++) {
		if (strcmp(cm[i], "sum") == 0) {
			cout << sum[0][2] << endl;
		} else if (strcmp(cm[i], "add") == 0) {
			update(0, 1, m, cp[arg[i]], 1);
		} else {
			update(0, 1, m, cp[arg[i]], -1);
		}
	}

	return 0;
}
```
