[题目链接](http://codeforces.com/problemset/problem/524/C)
【二分】【枚举】
##题目大意
有$n$种面值的货币，每种货币数量无穷。有$q$个询问，每个询问为使用不超过$k$张且不超过两种面值的货币，是否能得到给定的总金额。有解输出货币最少张数，无解输出$-1$。

##思路
因为$k$不大，所以将所有的$1, 2, \dots,  k$张相同的货币的值处理出来。
然后枚举其中的值，再二分剩下的，看是否那么个值存在。

##代码实现
```cpp
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>


using namespace std;
typedef long long ll;

const int N = 5e3 + 100;
const int INF = 0x3f3f3f3f;
const int K = 30;

vector<int> moneys[K];

int a[N], n, k;


int main(void) {
//	freopen("in.txt", "r", stdin);

	scanf("%d%d", &n, &k);
	for (int i = 0; i < n; i++) {
		scanf("%d", &a[i]);
	}

	moneys[0].push_back(0);
	for (int i = 1; i <= k; i++) {
	    for (int j = 0; j < n; j++) {
	        moneys[i].push_back(a[j] * i);
	    }
	}
	int q, need;
	scanf("%d", &q);
	while ( q -- ) {
	    int ans = INF;
	    scanf("%d", &need);
	    for (int i = 0; i <= k; i++) {
	        for (int j = 0; j < moneys[i].size(); j++) {
	            int res = need - moneys[i][j];
	            if ( res < 0 ) continue;
	            if ( res == 0 ) ans = min(ans, i);
	            for (int num = 1; num <= k; num ++) {
	                int pos = lower_bound(moneys[num].begin(), moneys[num].end(), res) - moneys[num].begin();
	                if ( pos < moneys[num].size() && moneys[num][pos] == res)
	                    ans = min(ans, i + num);
	            }
	       }
	   }
	   if ( ans> k ) puts("-1");
	   else printf("%d\n", ans);
    }

	return 0;
}
```
