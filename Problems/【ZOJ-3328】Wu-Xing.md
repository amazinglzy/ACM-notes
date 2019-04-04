[题目链接](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemCode=3328)
【构造】

##题目大意
求一个n阶完全图中的最少的圈的个数。

##思路
如果 $n$ 是奇数，那么 $n$ 的度数为 $n - 1$，所以其中的某一点在 (n - 1) / 2个圈里。
如果 $n$ 是偶数，那么加一个虚点，这样圈数为 $ n / 2 $，然后将虚点看成一条边就好。

##实现代码
```cpp
#include <cstdio>

using namespace std;

int main(void) {
	freopen("in.txt", "r", stdin);

	int n;
	while ( scanf("%d", &n), n) {
		if ( n & 1 ) {
			printf("%d\n", (n - 1) / 2);
			continue;
		} else {
			printf("%d\n", n / 2);
		}

	}


	return 0;
}
```
