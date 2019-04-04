# Suffix Automation

## Description
最简可以识别一个串所有后缀的自动机。

## Template
```cpp
const int N = 2e5 + 100;
const int M = 26;

struct SAM {
    int ch[N][M];
    int par[N];
    int max_len[N];
    int root, last, sz;

    void init() {
        sz = 0;
        root = sz ++;
        mem(ch[root], -1);
        par[root] = -1;
        max_len[root] = 0;
        last = root;
    }

    void extend(int x) {
        int p = last, np = sz ++;
        mem(ch[np], -1);
        max_len[np] = max_len[p] + 1;
        while (p != -1 && ch[p][x] == -1)
            ch[p][x] = np, p = par[p];
        if (p == -1)
            par[np] = root;
        else {
            int q = ch[p][x];
            if (max_len[q] == max_len[p] + 1)
                par[np] = q;
            else {
                int nq = sz ++;
                memcpy(ch[nq], ch[q], sizeof(ch[nq]));
                max_len[nq] = max_len[p] + 1;
                par[nq] = par[q];
                par[q] = nq;
                par[np] = nq;
                while (p != -1 && ch[p][x] == q)
                    ch[p][x] = nq, p = par[p];
            }
        }
        last = np;
    }

    int l[N];
    int ra[N];
    void count() {
        for (int i = 0; i <= max_len[last]; i++)
            l[i] = 0;
        for (int i = 0; i < sz; i++)
            l[max_len[i]] ++;
        for (int i = 1; i <= max_len[last]; i++)
            l[i] += l[i - 1];
        for (int i = sz - 1; i>= 0; i--)
            ra[--l[max_len[i]]] = i;

        for (int i = 0; i < sz; i++)
            printf("%d%c", ra[i], " \n"[i==sz-1]);
    }

    void debug() {
        for (int i = 0; i < sz; i++)
            for (int j = 0; j < M; j++) if (ch[i][j] != -1)
                printf("%d %c %d\n", i, j + 'a', ch[i][j]);
    }
} t;
```

## Problem
### [SPOJ SUBLEX](http://www.spoj.com/problems/SUBLEX/)
```cpp
# include <bits/stdc++.h>
using namespace std;

# define X first
# define Y second
# define sq(x) ((x)*(x))
# define mem(x,y) memset(x,y,sizeof(x))
# define cmin(x,y) ((x)=((x)<(y)?(x):(y)))
# define cmax(x,y) ((x)=((x)>(y)?(x):(y)))
# define IN(x1,y1,x2,y2) ((x2)<=(x1)&&(y1)<=(y2))
# define DV(x1,y1,x2,y2) ((y1)<(x2)||(y2)<(x1))
# define show(x) cerr << #x << " = " << x << endl
# define time cerr<<"\n\n"<<"Time Spec = "<<clock()/1000.<<"s\n"
typedef double DB;
typedef long long LL;
typedef pair<int, int> Pr;
typedef unsigned long long U;

const LL INF = 1e9 + 7;
const DB EPS = 1e-8;
const int N = 2e5 + 100;
const int M = 26;
const int MOD = 1e9 + 7;

char ans[N];

struct SAM {
    int ch[N][M];
    int par[N];
    int max_len[N];
    int root, last;
    int sz;

    int new_node( ) {
        for (int i = 0; i < M; i++)
            ch[sz][i] = 0;
        par[sz] = 0;
        return sz ++;
    }

    void cp_node(int t, int cp) {
        for (int i = 0; i < M; i++)
            ch[t][i] = ch[cp][i];
        max_len[t] = max_len[cp];
        par[t] = par[cp];
    }

    void init(char *s) {
        sz = 0;
        new_node();

        root = new_node();
        last = root;

        for (int i = 0; s[i]; i++)
            extend(s[i]);
    }

    void extend(char c) {
        int nxt = c - 'a';

        int p = last;
        int np = new_node( );
        max_len[np] = max_len[p] + 1;
        while (p && ch[p][nxt] == 0)
            ch[p][nxt] = np, p = par[p];
        if (p == 0)
            par[np] = root;
        else {
            int q = ch[p][nxt];
            if (max_len[q] == max_len[p] + 1)
                par[np] = q;
            else {
                int nq = new_node( );
                cp_node(nq, q);
                max_len[nq] = max_len[p] + 1;
                par[q] = nq;
                par[np] = nq;
                while (p && ch[p][nxt] == q)
                    ch[p][nxt] = nq, p = par[p];
            }
        }
        last = np;
    }

    int num[N];
    void init_num() {
        mem(num, -1);
        dfs(root);
    }

    int dfs(int v) {
        if (num[v] >= 0)
            return num[v];
        num[v] = 1;
        for (int i = 0; i < M; i++)
            if (ch[v][i])
                num[v] += dfs(ch[v][i]);
        return num[v];
    }

    void find(int k) {
        for (int i = 0, cur = root, j; k; i++) {
            for (j = 0; j < M; j++) if ( ch[cur][j] ) {
                if ( k <= num[ ch[cur][j] ])
                    break;
                else
                    k -= num[ ch[cur][j] ];
            }

            ans[i] = j + 'a';
            cur = ch[cur][j];
            k --;
            if (k == 0)
                ans[i + 1] = '\0';
        }
    }

    void debug() {
        show(sz);
        for (int i = 0; i < sz; i++)
            printf("%d\n", num[i]);
    }
} s;

char str[N];

int main() {
# ifdef owly
    freopen("in.txt", "r", stdin);
    // freopen("ou.txt", "w", stdout);
# endif

    scanf("%s", str);
    s.init(str);
    s.init_num();
    // s.debug();

    int q, k;
    scanf("%d", &q);
    while (q--) {
        scanf("%d", &k);
        s.find(k);
        printf("%s\n", ans);
    }

    return 0;
}
```
