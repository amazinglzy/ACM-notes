[题目链接](https://odzkskevi.qnssl.com/8c4ec1199ae8b24d3b6d0080cf581da5?v=1506434666)

## 分析
对于一个向量 $v = (d_1, d_2, \cdots)$ 有一个价值，它等于 $\sum u$，$u$ 满足条件为只有一个维度和 $v$ 相差 $1$．
题目满足这个价值会小于 $2^{63}-1$．
所以状态数不会太多，直接用 $map$ 存就可以了．

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
const int N = 110;
const int M = 52;

typedef vector<int> V;
map<V, LL> dp;
set<LL> ans;

int d, h;
LL get_val(V & a) {
    V tv = a;
    sort(tv.begin(), tv.end());
    if (dp.count(tv))
        return dp[tv];
    LL ret = 0;
//    sort(tmp.begin(), tmp.end());
    for (int i = 0; i < d; i++) if (tv[i]) {
        tv[i] --;
//        sort(tv.begin(), tv.end());
        ret += get_val(tv);
        tv[i] ++;
//        sort(tv.begin(), tv.end());
    }
    return dp[a] = ret;
}

V cur;
void dfs(int pos, int min_val, int max_val) {
    if (pos == d - 1) {
        cur.push_back(max_val);
        ans.insert(get_val(cur));
        cur.pop_back();
        return ;
    }
    for (int i = min_val; i <= max_val; i++) {
        cur.push_back(i);
        dfs(pos+1, i, max_val - i); cur.pop_back();
    }
}

int main(void) {
#ifdef owly
    freopen("data.in", "r", stdin);
#endif // owly
    scanf("%d%d", &d, &h);
    V tmp(d);
    dp.clear();
    dp[tmp] = 1;
    dfs(0, 0, h - 1);
    for (set<LL>::iterator it = ans.begin(); it != ans.end(); it++)
        printf("%lld\n", *it);

    return 0;
}
```
