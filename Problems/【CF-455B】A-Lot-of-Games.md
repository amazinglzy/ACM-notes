[题目链接](http://codeforces.com/problemset/problem/455/B)

##题目大意
* $n$ 个字符串
* 另一个字符串初始为空，两个人轮流选一个字符放最后，保证这个字符串是上面 $n$ 个字符串中的某个的前缀
* 当某个人无法继续时，另一个人便胜了
* 这样的游戏进行 $k$ 次
* 如果第$i$次$A$输了，第$i+1$次$A$先手
* 以最后一次胜负论输赢
* $(1 \leq n \leq 10^5, 1 \leq k \leq 10^9) $

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#define minv(x,y) ((x) < (y) ? (x) : (y))
#define maxv(x,y) ((x) > (y) ? (x) : (y))

using namespace std;
typedef double db;
typedef long long ll;
typedef unsigned int ui;

const int N = 1e5 + 100;
const int INF = 0x3f3f3f3f;

int trie[N][26], root, tot;
void insert(char *s) {
	int cur = root;
	for (int i = 0; i < strlen(s); i++) {
		int x = s[i] - 'a';
		if (trie[cur][x] == -1) {
			++ tot;
			trie[cur][x] = tot;
		}
		cur = trie[cur][x];
	}
}

int win[N], lose[N];
void dfs(int v) {
	win[v] = 0;
	lose[v] = 0;

	bool update = false;
	for (int i = 0; i < 26; i++) {
		if (trie[v][i] != -1) {
			dfs(trie[v][i]);
			win[v] |= !win[trie[v][i]];
			lose[v] |= !lose[trie[v][i]];
			update = true;
		}
	}

	if (!update) {
		win[v] = false;
		lose[v] = true;
	}
}

char s[N];

int main() {
//    freopen("in.txt", "r", stdin);
//    freopen("out.txt", "w", stdout);

	memset(trie, -1, sizeof(trie));  	
  	root = tot = 0;

  	int n, k;
  	scanf("%d%d", &n, &k);
  	for (int i = 0; i < n; i++) {
  		scanf("%s", s);
		insert(s);
  	}

  	dfs(root);
  	if (win[root] && !lose[root]) {
  		puts(k & 1 ? "First" : "Second");
	} else if (!win[root]) {
		puts("Second");
	} else {
		puts("First");
	}

	return 0;
}
```
