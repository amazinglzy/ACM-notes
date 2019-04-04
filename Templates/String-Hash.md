# String Hash
## Description
用hash函数将一个字符串及其各个前缀用一个数表示。

## Template
```cpp
const U seed = 31;
ha[0] = 1;
for (int i = 1; i <= len; i++)
    ha[i] = ha[i - 1] * seed + s[i - 1] - 'a';
for (int i = 0; i + L <= len; i++)
    h[i] = ha[i + L] - ha[i] * ba[L];
```
## Problem
### [HDU 4821](http://acm.hdu.edu.cn/showproblem.php?pid=4821) 模板题

```cpp
# include <bits/stdc++.h>
using namespace std;

# define X first
# define Y second
# define sq(x) ((x)*(x))
# define Mem(x,y) memset(x,y,sizeof(x))
# define cMin(x,y) ((x)=((x)<(y)?(x):(y)))
# define cMax(x,y) ((x)=((x)>(y)?(x):(y)))
# define In(x1,y1,x2,y2) ((x2)<=(x1)&&(y1)<=(y2))
# define Dv(x1,y1,x2,y2) ((y1)<(x2)||(y2)<(x1))
# define show(x) cerr << #x << " = " << x << endl
# define time cerr<<"\n\n"<<"Time Spec = "<<clock()/1000.<<"s\n"
typedef double DB;
typedef long long LL;
typedef pair<int, int> Pr;
typedef unsigned long long U;

const LL INF = 1e9 + 100;
const DB EPS = 1e-8;
const int N = 1e5 + 100;
const int MOD = 998244353;

int M, L;
char s[N];

const U seed = 31;
U ba[N], ha[N], h[N];
map<U, int> cnt;

int main() {
# ifdef owly
    freopen("in.txt", "r", stdin);
    // freopen("ou.txt", "w", stdout);
# endif

    ba[0] = 1;
    for (int i = 1; i < N; i++)
        ba[i] = seed * ba[i - 1];

    while (~scanf("%d%d", &M, &L)) {
        scanf("%s", s);
        int len = strlen(s);

        ha[0] = 1;
        for (int i = 1; i <= len; i++)
            ha[i] = ha[i - 1] * seed + s[i - 1] - 'a';
        for (int i = 0; i + L <= len; i++)
            h[i] = ha[i + L] - ha[i] * ba[L];

        int ans = 0;
        for (int i = 0; i < L && i + M * L < len; i++) {
            cnt.clear();
            for (int j = 0; j < M; j++) {
                cnt[ h[i + j * L] ] ++;
            }
            if ((int)cnt.size() == M)
                ans ++;
            for (int j = M; i + (j + 1) * L <= len; j++) {
                cnt[ h[i + j * L] ] ++;
                cnt[ h[i + (j - M) * L] ] --;
                if (cnt[ h[i + (j - M) * L] ] == 0)
                    cnt.erase( h[i + (j - M) * L] );
                if ((int)cnt.size() == M)
                    ans ++;
            }
        }
        printf("%d\n", ans);
    }

    return 0;
}

```
