[题目链接](http://codeforces.com/problemset/problem/496/D)
【调和级数】

##题目大意
进行若干场比赛，每次比赛两人对决，赢的人得到1分，输的人不得分，先得到t分的人获胜，开始下场比赛，某个人率先赢下s场比赛时，游戏结束。
现在给出n次对决的记录，问可能的s和t有多少种，并按s递增的方式输出。

##思路
需要将所有的(s, t)都找到。
最暴力的算法是枚举t的值，然后算是否能构成一次比赛。但这样看起来是$O(n^2)$
但是，$n + \frac {n} {2} + \frac {n} {3} + \dots = nlog(n)$
所以只要预处理下，复杂度便合适了。

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <vector>
#include <iostream>
#include <algorithm>

using namespace std;
typedef pair<int, int> P;

const int N = 1e5 + 100;
int a[N], n;

P petya[N], gena[N];
vector<P> ans;

int main(void) {
//	freopen("in.txt", "r", stdin);

	int sp = 0, sg = 0;
	scanf("%d", &n);
	for (int i = 0; i < n; i++) {
		scanf("%d", &a[i]);
		if ( a[i] == 1 ) {
			sp ++;
			petya[sp] = P(i, sg);
		} else {
			sg ++;
			gena[sg] = P(i, sp);
		}
	}

	int tmax = max(sp, sg);
	int spmax = sp, sgmax = sg;
	for (int i = 1; i <= tmax; i++) { P p1, p2; sp = 0, sg = 0; int pwin = 0, gwin = 0; while ( true ) { if (sp + i> spmax && sg + i > sgmax ) break;
			if (sp + i > spmax ) {
				while (sg + i <= sgmax ) sg = sg + i, gwin ++; if ( sg == sgmax && gwin> pwin && gena[sg].first == n-1) {
					ans.push_back(P(gwin, i));
				}
				break;
			}
			if (sg + i > sgmax ) {
				while ( sp + i <= spmax ) sp = sp + i, pwin ++; if ( sp == spmax && pwin> gwin && petya[sp].first == n-1) {
					ans.push_back(P(pwin, i));
				}
				break;
			}

			p1 = petya[sp + i], p2 = gena[sg + i];
			if ( p1.first > p2.first ) {
				sg = sg + i;
				sp = gena[sg].second;
				gwin ++;
			} else {
				sp = sp + i;
				sg = petya[sp].second;
				pwin ++;
			}
		}
	}

	printf("%d\n", (int)ans.size());
	sort(ans.begin(), ans.end());
	for (int i = 0; i < ans.size(); i++) {
		printf("%d %d\n", ans[i].first, ans[i].second);
	}

	return 0;
}
```
