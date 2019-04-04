[题目链接](http://codeforces.com/contest/359/problem/D)
【尺取法】

##题目大意
给一个$n$个数的数组，求$[l, r]$使得存在$j(l <= j <= r)$，使得$[l, r]$里的数可以被$a_j$整除。 ##思路 直接往后扫一遍就好。 因为要找的那个数是区间里最小的，所以当扫的时候，遇见一个小的数不在区间内，就将指向除数的指针指向它。因除数不可能是它之前的数。如果是，那么区间一定小于前后确定的指针指向的两个数之间的区间。因为这个数一定比这两个数要大。 ##代码实现 ```cpp #include <cstdio>
#include <vector>
#include <algorithm>

using namespace std;

int gcd(int a, int b) {
	return b == 0 ? a : gcd(b, a % b);
}

const int N = 3e5 + 100;
int a[N], n;
vector<int> ans;


int main(void) {
//	freopen("in.txt", "r", stdin);

	scanf("%d", &n);
	for (int i = 0; i < n; i++) {
		scanf("%d", a + i);
	}

	int maxlen = 0;
	int l = 0, r = 0;
	while ( l < n ) {
		int stamp = l;
		while ( r + 1 < n && gcd(a[stamp], a[r + 1]) == a[stamp]) r ++;
		while ( l - 1 >= 0 && gcd(a[stamp], a[l - 1]) == a[stamp]) l --;
		if ( r - l > maxlen ) {
			maxlen = r - l;
			ans.clear();
			ans.push_back(l);
		} else if ( r - l == maxlen ) {
			ans.push_back(l);
		}
		l = r + 1;
	}
	printf("%d %d\n", (int)ans.size(), maxlen);
	sort(ans.begin(), ans.end());
	for (int i = 0; i < ans.size(); i++) {
		printf("%d ", ans[i] + 1);
	}

	return 0;
}
```
