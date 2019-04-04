[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6000)
## 题目大意
$L$ 件衣服，$n$ 台洗衣机，每台洗衣机洗完一件衣服的时间 $w_i$，$m$ 台烘干机，每台烘干机烘干一件衣服所需要的时间 $d_i$；
求将这 $n$ 件衣服洗完后烘干的最少时间是多少？
$n$ 台洗衣机和 $m$ 台烘干机可以同事工作。

## 限制条件
$1 \leq T \leq 100$
$1 \leq L \leq 1e6$
$1 \leq N, M \leq 1e5$
$1 \leq w_i, d_i \leq 1e9$

## 思路
只考虑洗衣机，可以贪心的算出每一件衣服洗完的时间，而对于烘干机来说，如果这些烘干机要在时刻 $x$ 内烘干完所有的衣服，我们也可以算出对于每件衣服至少要在某一个时刻之前开始烘干。

## 代码实现
```cpp
# include <bits/stdc++.h>

using namespace std;
# define cmin(x,y) ((x)=min(x,y))
# define cmax(x,y) ((x)=max(x,y))
# define sq(x) ((x)*(x))
# define x first
# define y second
# define show(x) cerr<<#x<<" = "<<x<<endl typedef pair<int, int> Pr;
typedef double DB;
typedef long long LL;
const int N = 1e5 + 100;
const int M = 1e6 + 100;


LL wash[M], cw;
LL dry[M], dw;
int w[N], d[N];
int l, n, m;

struct node {
    LL x;
    int y;
    node(LL x, int y) : x(x), y(y) {}
};

bool operator<(node a, node b) {return a.x> b.x; }

priority_queue<node> que;

int main(void) {
#ifdef owly
    freopen("a.in", "r", stdin);
    //freopen("a.out", "w", stdout);
#endif // owly

    int T, o = 1;
    scanf("%d", &T);
    while (T --) {
        scanf("%d%d%d", &l, &n, &m);
        for (int i = 0; i < n; i++)
            scanf("%d", &w[i]);
        for (int i = 0; i < m; i++)
            scanf("%d", &d[i]);

        while (!que.empty())
            que.pop();
        for (int i = 0; i < n; i++)
            que.push(node(w[i], i));

        int cw = 0;
        for (int i = 0; i < l; i++) {
            node ch = que.top(); que.pop();
            wash[cw ++] = ch.x;
            ch.x += w[ch.y];
            que.push(ch);
        }

        while (!que.empty())
            que.pop();
        for (int i = 0; i < m; i++)
            que.push(node(d[i], i));
        int dw = 0;
        for (int i = 0; i < l; i++) {
            node ch = que.top(); que.pop();
            dry[dw ++] = ch.x;
            ch.x += d[ch.y];
            que.push(ch);
        }

        LL ans = 0;
        for (int i = 0; i < l; i++)
            cmax(ans, wash[i] + dry[l - i - 1]);
        printf("Case #%d: %lld\n", o++, ans);
    }

    return 0;
}

```
