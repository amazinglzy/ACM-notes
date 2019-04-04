[题目链接](http://acm.split.hdu.edu.cn/showproblem.php?pid=5942)
## 题目大意
令 $f(x)$ 是 $x$ 的质因子的个数，$g(x) = 2^{f(x)}$ ，求 $\sum \limits _{i=1}^n g(i)$。

## 分析
对于一个 $x = p_1^{c_1}p_2^{c_2} \cdot p_n^{c_n}$，$2^n$ 就是 $p \cdot q = x, gcd(p, q) = 1$ 的 $p$ 和 $q$ 的对数。
所以题目要求的就是
$$\sum \limits_{p, q} [1 \leq p, q \leq n][p \cdot q \leq n][gcd(p, q) = 1]$$
因为 $\sum \limits_{p,q} [1 \leq p, q \leq n] = \sum \limits_{p,q} [1 \leq p \lt q \leq n] + \sum \limits_{p,q} [1 \leq q \lt p \leq n] + \sum \limits_{p,q} [1 \leq p = q \leq n]$
且 $\sum \limits_{p,q} [1 \leq p = q \leq n][p \cdot q \leq n][gcd(p, q) = 1] = 1$
剩下的两部分是对称的，所以原式为
$$1 + 2 \sum \limits_{p,q} [1 \leq p \lt q \leq n][p \cdot q \leq n][gcd(p,q) = 1]$$
$$\sum \limits_{p,q} [1 \leq p \lt q \leq n][p \cdot q \leq n][gcd(p,q) = 1] = \sum \limits_{p} [1 \leq p \lt \sqrt{n}] \sum \limits_q [p \lt q \leq \frac {n} {p}][gcd(p,q) = 1]$$
令 $f(x) = \sum \limits_q [1 \leq q \leq x][gcd(p,q) = 1]$
则原式为 $f(\frac {n} {p}) - f(p - 1)$，这一部分用容斥，求 $[1, p]$ 与 $q$ 互质的数的个数，相当于求 $[1, p]$ 中与 $q$ 不互质的数的个数。

> 求 $[1, n]$ 中与 $x$ 不互质的数的个数
> $x = p_1^{c_1}p_2^{c_2} \cdot p_k^{c_k}$，分别求出 $[1,n]$ 中是 $S$ 的倍数的数的个数($S$是从$p$中选一些出来)，这样就可以用容斥算出来了。

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
const int N = 1e5 + 100;
const int M = 1e6 + 100;
const int MOD = 1e9 + 7;

vector<int> p[M];
LL f(LL x, LL q) {
    LL cq = q;
    int m = p[q].size();

    LL ret = 0;
    for (LL s = 1; s < (1 << m); s++) { LL cof = -1, sn = 1; for (int j = 0; j < m; j++) if ((s>> j) & 1) cof *= -1, sn *= p[q][j];
        ret += cof * (x / sn);
    }
    return x - ret;
}

LL n;

int main(void) {
#ifdef owly
    freopen("in.txt", "r", stdin);
    //freopen("out.txt", "w", stdout);
#endif // owly
    for (int i = 1; i < M; i++) {
        int j = i;
        for (int k = 2; k * k <= j; k ++) { if (j % k != 0) continue; p[i].push_back(k); while (j % k == 0) j /= k; } if (j> 1) p[i].push_back(j);
    }

    int T, cnt = 1;
    scanf("%d", &T);
    while (T --) {
        scanf("%lld", &n);
        LL lim = sqrt(n);
        if (lim * lim != n) lim ++;
        LL ans = 0;
        for (LL p = 1; p < lim; p ++) {
            ans += f(n/p, p) - f(p, p);
            ans %= MOD;
            if (ans < 0) ans += MOD;
        }
        ans = (1 + 2*ans) % MOD;
        printf("Case #%d: %lld\n", cnt ++, ans);
    }


    return 0;
}
```
