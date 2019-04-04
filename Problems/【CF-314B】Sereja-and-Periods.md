[题目链接](http://codeforces.com/contest/314/problem/B)
【KMP】

## 题目大意
给两个字符串，形式是一个子串和其重复次数。
求第二个字符串最多重复多少次仍是第一个字符串子序列。

## 思路

类似于$KMP$的思路。

## 代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <set>
#include <map>
#include <queue>

#define minv(x,y) ((x) < (y) ? (x) : (y))
#define maxv(x,y) ((x) > (y) ? (x) : (y))
using namespace std;

const int N = 200;
char a[N], c[N];
int cnt[N], nxt[N];
int b, d;

int main(void) {
//	freopen("in.txt", "r", stdin);

	scanf("%d%d", &b, &d);
	scanf("%s%s", a, c);

	int len_a = strlen(a);
	int len_c = strlen(c);

	for (int i = 0; i < len_c; i++) {
		int now = i;
		for (int j = 0; j < len_a; j++) {
			if (c[now] == a[j]) {
				now ++;
				if (now >= len_c) {
					++cnt[i];
					now = 0;
				}
			}
		}
		nxt[i] = now;
	}

	int sum = 0;
	for (int i = 0, now = 0; i < b; i++, now = nxt[now]) sum += cnt[now];
	printf("%d\n", sum / d);

	return 0;
}

```
