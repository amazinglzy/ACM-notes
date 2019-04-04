[题目链接](http://codeforces.com/problemset/problem/128/B)
【后缀数组】

##题目大意
给一个字符串，求出其第k大的前缀。

##思路
在后缀数组里先向下扫，再向右扫。

##代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;
typedef long long ll;

const int N = 1e5 + 100;
int ra[N], tmp[N];
int n, k;
bool compare_sa(int i, int j) {
	if ( ra[i] != ra[j] ) return ra[i] < ra[j];
	int ri = i + k <= n ? ra[i + k] : -1;
	int rj = j + k <= n ? ra[j + k] : -1;
	return ri < rj;
}
char s[N];
int sa[N];
void construct_sa( ) {
    n = strlen(s);
    for (int i = 0; i <= n; i++) {
        sa[i] = i;
        ra[i] = i < n ? s[i] : -1;
    }
    for (k = 1; k <= n; k *= 2) {
        sort(sa, sa + n + 1, compare_sa);
        tmp[sa[0]] = 0;
        for (int i = 1; i <= n; i++) {
            tmp[sa[i]] = tmp[sa[i - 1]] + (compare_sa(sa[i - 1], sa[i]) ? 1 : 0);
        }
        for (int i = 0; i <= n; i++) {
            ra[i] = tmp[i];
        }
    }
}

int he[N];

void generate_he( ) {
    int i, j, k = 0;
    for (i = 1; i <= n; i++) ra[sa[i]] = i;
    for (i = 0; i < n; he[ra[i ++]] = k)
        for (k ? k--:0, j = sa[ra[i] - 1]; s[i + k] == s[j + k]; k++);
    return;
}
int x;
char ans[N];
bool check(int ok, int i, int j) {
    if ( ! ok && j>= 0) {
		for (int b = 0; b <= j; b++)
		    printf("%c", s[sa[i] + b]);
		printf("\n");
		return true;
	}
	return false;
}

int main(void) {
// freopen("in.txt", "r", stdin);
    scanf("%s%d", s, &x);
    construct_sa();
    /* for (int i = 1; i <= n; i++) { printf("%s\n", &s[sa[i]]); } printf("\n"); */
    generate_he();
    int j = 0;
    for (int i = 1; i <= n; i++) {
        for (j; j < n - sa[i]; j++, x--) {
            if ( check (x, i, j - 1) )
                return 0;
            int imax = i;
            while ( he[imax + 1]> j ) {
				imax ++; x--;
				if ( check (x, imax - 1, j) ) return 0;
			}
		}
		if (check (x, i, j - 1) ) return 0;
		j = he[i + 1];
	}
	printf("No such line.");


	return 0;
}
```
