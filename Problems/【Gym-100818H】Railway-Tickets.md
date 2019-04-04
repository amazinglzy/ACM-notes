[题目链接](https://vjudge.net/problem/281315/origin)
## 题目大意
一列火车有 $k$ 个座位和 $n$ 个站点，现有一些车票信息，包括 $pl, st, fn$ 分别表示座位号，起始站点和下车站点，保证输入合法。
定义 $(p, q)$ 可直接到达 指存在一个座位在 $p \to q$ 这个站点区间中没有被买过（对应给出的车票），求有多少个区间是不可直接到达且可以通过买一些火车票可以能够覆盖这个区间。

## 分析
假设没有不可直接到达的条件，考虑以第 $i$ 个位置为终点的区间，那么这类的区间数是 $i - j$，$j$是指离 $i$ 最远的一个位置且从 $j$ 可以到达 $i$。这种情况下，只要座位没有坐满就是可以到达的。

考虑可直接到达的情况，以第 $i$ 个位置为终点的区间数，也为 $i - j$，$j$ 是离 $i$ 最近的一个位置且 $j \to i$ 这个区间有一个座位可以直接到达。

对于每个位置算出这两个数，将差加到答案里就可以了。

## 代码实现
```cpp
# include <bits/stdc++.h>

using namespace std;
# define cmin(x,y) ((x)=min(x,y))
# define cmax(x,y) ((x)=max(x,y))
# define sq(x) ((x)*(x))
# define x first
# define y second
# define orz(x) cerr<<#x<<" = "<<x<<endl typedef pair<int, int> Pr;
typedef double DB;
typedef long long LL;
const int N = 1e4 + 100;
const int M = 1e3 + 100;

int k, n, t;
int f[M][N];
int max_len[N], pre_len[N];

int main(void) {
#ifdef owly
    freopen("in.txt", "r", stdin);
    //freopen("out.txt", "w", stdout);
#endif // owly

    memset(f, 0, sizeof(f));
    memset(max_len, 0, sizeof(max_len));
    memset(pre_len, 0, sizeof(pre_len));

    scanf("%d%d%d", &k, &n, &t);

    int pl, st, fn;
    for (int i = 0; i < t; i++) {
        scanf("%d%d%d", &pl, &st, &fn);
        f[pl][st] ++, f[pl][fn] --;
        max_len[st] ++, max_len[fn] --;
    }

    for (int j = 2; j <= n; j++) {
        max_len[j] += max_len[j - 1];
        for (int i = 1; i <= k; i++) f[i][j] += f[i][j - 1];
    }

    for (int j = 1; j <= n; j++) {
        max_len[j] = (max_len[j] != k);
        for (int i = 1; i <= k; i++) f[i][j] = f[i][j] ^ 1;
    }

    for (int j = 2; j <= n; j++) {
        if (max_len[j]) max_len[j] += max_len[j-1];
        for (int i = 1; i <= k; i++) if (f[i][j]) f[i][j] += f[i][j-1];
    }
    for (int j = 1; j <= n; j++)
        for (int i = 1; i <= k; i++) cmax(pre_len[j], f[i][j]);

    LL ans = 0;
    for (int j = 1; j < n; j++) ans += max_len[j] - pre_len[j];
    cout << ans << endl;

    return 0;
}
```
