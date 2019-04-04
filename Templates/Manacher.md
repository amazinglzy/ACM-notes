# Manacher

## Description
用动态规划的思想来求解字符串中以每一个字符为中心的回文串的最大长度

## Template
```cpp
const int N = 1e6 + 100;

char tmp[2 * N];
int r[2 * N];
int manacher(char *s) {
    int len = strlen(s);
    for (int i = 0; i < len; i++) {
        tmp[2 * i + 1] = '#';
        tmp[2 * i + 2] = s[i];
    }
    tmp[0] = '@';
    tmp[2 * len + 1] = '#';
    tmp[2 * len + 2] = '\0';
    len = 2 * len + 2;
    // printf("%s\n", tmp);

    int id, mx = 0, ret = 0;
    for (int i = 0; i < len; i++) {
        r[i] = i < mx ? Min(r[2 * id - i], mx - i) : 1;
        while (tmp[i + r[i]] == tmp[i - r[i]]) r[i] ++;
        if (r[i] + i > mx) {
            mx = r[i] + i;
            id = i;
        }
        ret = Max(ret, r[i]);
    }

    // for (int i = 0; i < 2 * len + 1; i++) {
    //     printf("%d ", r[i]);
    // }
    return ret - 1;
}
```

## Problems

### [HDU 3068](http://acm.hdu.edu.cn/showproblem.php?pid=3068)
**思路** manacher 裸题
```cpp
# include <bits/stdc++.h>
using namespace std;

# define sq(x) ((x)*(x))
# define X first
# define Y second
# define Min(x,y) ((x)<(y)?(x):(y))
# define Max(x,y) ((x)>(y)?(x):(y))
# define Mem(x,y) memset(x,y,sizeof(x))
# define In(x1,y1,x2,y2) ((x2)<=(x1)&&(y1)<=(y2))
# define Dj(x1,y1,x2,y2) ((y1)<=(x2)||(y2)<=(x1))
typedef double DB;
typedef long long LL;
typedef pair<int, int> P;

const LL INF = 1e9 + 100;
const DB EPS = 1e-8;
const int N = 1e6 + 100;
const int M = 300;
const int MOD = 1e9 + 7;

char tmp[2 * N];
int r[2 * N];
int manacher(char *s) {
    int len = strlen(s);
    for (int i = 0; i < len; i++) {
        tmp[2 * i + 1] = '#';
        tmp[2 * i + 2] = s[i];
    }
    tmp[0] = '@';
    tmp[2 * len + 1] = '#';
    tmp[2 * len + 2] = '\0';
    len = 2 * len + 2;
    // printf("%s\n", tmp);

    int id, mx = 0, ret = 0;
    for (int i = 0; i < len; i++) {
        r[i] = i < mx ? Min(r[2 * id - i], mx - i) : 1;
        while (tmp[i + r[i]] == tmp[i - r[i]]) r[i] ++;
        if (r[i] + i > mx) {
            mx = r[i] + i;
            id = i;
        }
        ret = Max(ret, r[i]);
    }

    // for (int i = 0; i < 2 * len + 1; i++) {
    //     printf("%d ", r[i]);
    // }
    return ret - 1;
}

char s[N];

int main() {
    // freopen("in.txt", "r", stdin);

    while(~scanf("%s", s)){
        printf("%d\n", manacher(s));
    }

    return 0;
}
```
