[题目链接](http://codeforces.com/problemset/problem/732/D)
【二分】【贪心】

##题目大意
m门考试，每门考试需要$d_i$天的时间来准备。
n天，每天只能考规定的考试（可以规定不考），也可以复习。
问最少要多少天来考完考试，考不完输出 -1。

##分析
首先，记录学习的天数，到考试的时候再使用。
找一个1到m的序列s，使其位置满足$pos_i \geq day_{left} + s_k + 1$
所以，可以从后找这个序列，这样的天数是最多的。从后扫一遍，如果这天是考试，就从学习的天数中拿出一定的天数来，否则这天学习，这是需要贪心的将这天的学习用在已经考过的考试上，使得可用的天数更多。

##代码实现
```cpp
#include <cstdio>

using namespace std;

const int N = 1e5 + 100;

int d[N], a[N];
int n, m, tot;

bool f[N];
bool ok(int days) {
	for (int i = 0; i <= m; i++) f[i] = true;
	int res = m, sum = days - m;
	int add = 0;
	for (int i = days - 1; i>= 0; i--) {
		if ( sum < 0 ) return false;
		if ( res == 0 ) return true;
		if ( d[i] == 0 || f[d[i]] == false) {
			if ( add ) add --;
			else sum --;
		} else if ( f[d[i]] == true ) {
			f[d[i]] = false;
			add += a[d[i]];
			sum -= a[d[i]];
			res --;
		}
	}
	return false;
}

int main(void) {
//	freopen("in.txt", "r", stdin);

	scanf("%d%d", &n, &m);
	for (int i = 0; i < n; i++) {
		scanf("%d", &d[i]);
	}

	tot = 0;
	for (int i = 1; i <= m; i++) {
	    scanf("%d", &a[i]);
	    tot += a[i];
	}
	int left = -1, right = n;
	while ( right - left> 1 ) {
		int mid = (left + right) / 2;
		if ( ok( mid ) ) {
			right = mid;
		} else {
			left = mid;
		}
	}

//	printf("%s\n", ok(3) ? "YES" : "NO" );

	if ( ok(right) ) printf("%d\n", right);
	else printf("-1\n");

	return 0;
}
```
