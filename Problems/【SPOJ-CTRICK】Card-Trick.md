[题目链接](http://www.spoj.com/problems/CTRICK/)
## 题目大意
[题目描述过程视频]( https://www.youtube.com/watch?v=kP7FJGzorpI)
求一个排列，第 $i$ 次，将 $i$ 个数移到这个排列的后面，一次移动一个数，这时排列最前面的数是 $i$，并去掉这个数。（$i$ 从 $1$ 开始）

**倒着模拟可以理解为**

- 一个圆排列，初始有一个元素值为 $n$，有一个指针指向这个元素。
- 进行 $n - 1$ 次操作，第 $i$ 次操作在这个指针的左边加入值为 $n - i$ 的元素，并将这个指针左移 $n - i + 1$ 个位置。（$i$ 从 $1$ 开始）。
- 答案就是从这个位置往后遍历这个顺序遍历的元素的集合。

需要一个数据结构来维护如下功能：

- 查询第 $k$ 个数，复杂度为至少为 $O(logn)$。
- 插入一个数，复杂度也至少为 $O(logn)$。

可以用平衡树来做。

**换一种思路**
正向模拟，用 $[1,n]$ 这 $n$ 个数填 $n$ 个空。设当前位置为 $1$，现开始数数，填第 $i$ 个空时，先数 $i$ 个空，第 $i+1$ 个空填上 $i$ 这个数。
这样，用一个数据结构算出一个区间的空的个数就可以模拟这个过程，可以用树状数组或者线段树就可以。

## 代码实现(BIT)
```cpp
# include <bits/stdc++.h>
using namespace std;
# define orz(x) cerr<<#x<<" = "<<x<<endl
# define sq(x) ((x)*(x))
# define x first
# define y second
typedef double DB;
typedef long long LL;
typedef pair<int, int> Pr;
const DB EPS = 1e-8;
const int INF = 0x3f3f3f3f;
const int N = 2e4 + 100;

int sum[N], n;
void add(int p, int x) {
    for ( ; p <= n; p += p & (-p)) sum[p] += x;
}
int prefix_sum(int p) {
    int ret = 0;
    for ( ; p> 0; p -= p & (-p))
        ret += sum[p];
    return ret;
}

void init() {
    for (int i = 0; i <= n; i++)
        sum[i] = 0;
    for (int i = 1; i <= n; i++)
        add(i, 1);
}

int find_pos(int k, int left, int right) {
    int base = left;
    while (right - left> 1) {
        int mid = (left + right) / 2;
        if (prefix_sum(mid) - prefix_sum(base) >= k) right = mid;
        else left = mid;
    }
    return right;
}

int ans[N];
int main(void) {
# ifdef owly
    freopen("in.txt", "r", stdin);
# endif

    int T;
    scanf("%d", &T);
    while (T --) {
        scanf("%d", &n);
        init();
        for (int i = 1; i <= n; i++) ans[i] = 0;

        int pre = 0;
        for (int i = 1; i <= n; i++) {
            int need = i;
            int res = n - i + 1;
            need %= res;
            need ++;
            int num_af = prefix_sum(n) - prefix_sum(pre);
//            orz(num_af);

            int pos;
            if (num_af < need) {
                need -= num_af;
                pos = find_pos(need, 0, pre);
            } else
                pos = find_pos(need, pre, n);
            ans[pos] = i;
            add(pos, -1);
            pre = pos;
        }
        for (int i = 1; i <= n; i++) printf("%d%c", ans[i], " \n"[i==n]);
    }

    return 0;
}
```
