[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=1358)
【KMP】【周期性判定】

##题目
给定一个字符串，求其前缀是否有周期，并将其长度和其周期数以长度从小到大的顺序输出。

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1e6 + 100;

char t[N];
int m;
int nxt[N];

void kmp() {
	memset(nxt, 0, sizeof(nxt));
	for (int i = 1; i < m; i++) {
		int j = i;
		while ( j > 0) {
			j = nxt[j];
			if ( t[i] == t[j] ) {
				nxt[i + 1] = j + 1;
				break;
			}
		}
	}

}


int main(void) {
//	freopen("in.txt", "r", stdin);

	int cnt = 1;
	while ( scanf("%d", &m) , m) {

		scanf("%s", t);

		kmp();

		printf("Test case #%d\n", cnt ++);
		for (int i = 1; i <= m; i++) { if ( i % (i - nxt[i]) == 0 && i / (i - nxt[i])> 1 ) {
				printf("%d %d\n", i, i / (i - nxt[i]));
			}
		}
		printf("\n");
	}


	return 0;
}
```
