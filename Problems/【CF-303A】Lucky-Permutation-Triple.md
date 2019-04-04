[题目链接](http://codeforces.com/contest/303/problem/A)
【构造】

##题目大意
给一个数$n$，求$3$个$n$的排列$\{ a_i \}, \{ b_i \}, \{ c_i \}$
且使得$a_i + b_i \equiv c_i$

##思路
发现当$n$是奇数时，排列错开一位，分别是$\{ a\}, \{ b \} $，当$n$是偶数时，这种情况是非法的。


##代码实现
```cpp
#include <cstdio>
#include <iostream>

using namespace std;

int main(void) {
//	freopen("in.txt", "r", stdin);

	int n;
	scanf("%d", &n);
	if (n % 2 == 0) puts("-1");
	else {
		for (int i = 0; i < n; i++) {
			printf("%d%c", i, i == n - 1 ? '\n' : ' ');
		}

		for (int i = 0; i < n; i++) {
			printf("%d%c", (i + 1) % n, i == n - 1 ? '\n' : ' ');
		}

		for (int i = 0; i < n; i++) {
			printf("%d%c", (2 * i + 1) % n, i == n - 1 ? '\n' : ' ');
		}
	}

	return 0;
}
```
