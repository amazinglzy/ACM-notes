# Summary
## 1001 Rikka with Candies
[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6085)
**题目大意**
已知 $A[i]$ 和 $B[i]$
$q$ 个询问 $k_i$ ，求 $$\sum_{i = 1}^{n} \sum_{j = 1}^{m} [A[i] \% B[j] = k_i]$$

**限制条件**
$1 \leq n, m, q \leq 50000$
$1 \leq A[i], B[i] \leq 50000$
如果 $i \neq j$ 则 $A[i] \neq A[j], B[i] \neq B[j]$

**思路**
暴力求，用bitset来维护 $A[i]$ 的值，即在bitset的位置为 $A[i]$ 上置 $1$，枚举左移的位数 $k$，将大于 $k$ 的 $B[j]$ 的 $0...n$ 倍在另一个bitset上对应位置上异或一下，答案就是第一个bitset左移 $k$ 位后与第二个bitset与之后的 $1$ 的个数。
