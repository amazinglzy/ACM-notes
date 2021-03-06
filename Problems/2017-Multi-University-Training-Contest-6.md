# Summary
## 1001 String
**题目大意**
给定 $n$ 个字符串，$m$ 个前缀和后缀，对于每一个前缀和后缀，求这 $n$ 个字符串中，满足前缀和后缀是给定的，且前缀和后缀不会相交的字符串的个数。

**限制条件**
$1 \leq n, m \leq 100000$
$1 \leq s_i \leq 100000$
$1 \leq \sum{s_i} \leq 500000$

**思路**
考虑对这 $n$ 个字符串排序以及这 $n$ 个字符串反串排序，对于一个字符串可以映射成二维平面的一个点，分别是这个字符串在上述两个序列中出现的位置。
对于一个前缀后缀，在序列中表示一个范围，所以每个前缀和后缀加起来表示一个矩形，这样题目转化为给定 $n$ 个点，以及 $m$ 个矩形，求每一个矩形中包含的点数。

在 trie 树上建立线段树可以解决这个问题。对于 $n$ 个字符串的 trie 树，每一个节点以及其儿子节点表示前缀相同的字符串的集合，在这个点上建立一棵线段树，维护该节点及子节点在反串序列的位置，这样一个区间查询就是要求的答案。

----------

## 1007 GCDispower
**题目大意**
给定一个长度为 $n$ 的排列 $p_i$，$q$ 次查询 $(l, r)$，求出
$$
\sum_{i=l}^{r}\sum_{j=i+1}^{r}\sum_{k=j+1}^{r} [gcd(p_i, p_j) = p_k] \cdot p_k
$$

**限制条件**
$1 \leq n \leq 100000$
$1 \leq l \leq r \leq 100000$

**思路**
题目是求对于每一个满足 $i \lt j \lt k, gcd(p_i, p_j) = p_k$ 的三元组的贡献和，每一个三元组的贡献为 $p_k$。
枚举 $p_k$，简化问题为对于 $p_k$，满足 $i \lt j, gcd(p_i, p_j)=p_k$ 的二元组的个数。


$[gcd(p_i, p_j) = p_k] = [gcd(\frac {p_i} {p_k}, \frac {p_j} {p_k} ) = 1]\ (p_k | p_i, p_j)$，否则就是 $0$，那么对于 $[l, k]$ 中是 $p_k$ 倍数的那些数，用 $p_k$ 除这些数，然后求剩下的这些数中互质的对数，设为 $g(l, k)$。

设查询 $(l, r)$ 的答案为 $f(l, r)$，那么 $f(l, r) = f(l, r - 1) + g(l, r)$。

如果 $l$ 不是 $p_k$ 的倍数，那么 $g(l, r) = g(l+1, r)$，这样就可以定义 $h(l, r)$为 $i = l, k = r, 1 \lt j \lt r$ 时满足 $gcd(\frac {p_i} {p_k}, \frac {p_j} {p_k}) = 1 = gcd(\frac {p_i} {p_k}, p_j)$ 的 $j$ 的个数。

>求 $[1..x]$ 中与 $n$ 互质的数的个数，可以用容斥来做。用容斥可以得到在这个区间内所有的数中有大于 $1$ 的约数的个数，这个区间内所有的数的个数减去这个数就是互质的数的个数。

如果能够处理出 $[l+1, r-1]$ 中 $\frac {a_i} {a_k}$ 的每个因子的倍数的个数，那么就可以用容斥求出 $j$ 的个数。

具体来说，就是枚举 $p_k$，并对于 $1 \leq l \leq k$，求出所有的 $f(l, k)$，而要求 $f(l, k)$，则从 $k - 1$ 向后枚举 $l$，求出所有的 $h(l, k)$，$g(l, k)$ 就是 $\sum_{i = l}^{k} g(i, k)$，这样就可以求出所有的答案了。

枚举 $p_k$ 以及对于每个 $p_k$ 枚举 $l$ 的过程是 $O(n^2)$ 的，对此，因为对于每一个 $p_k$，只有 $p_k$ 的倍数的集合才会对答案产生贡献，所以预处理出每个 $p_k$ 的倍数，以它们的位置。因为只用到了 $p_k$ 之前的数，所以需要对这些个位置排序，枚举的时候，二分找到 $p_k$ 的位置，就可以直接依次访问到离 $p_k$ 距离最近的 $p_l$了。这样复杂度是 $O(nlogn)$。

处理 $p_l$ 的时候，需要质因数分解，然后跑一个容斥，这部分复杂度是 $O(logn)$ 的。而对于 $h(l, k)$ 求和，也就是求 $g(l, k)$ 必须是 $O(logn)$ 的复杂度才可以过，所以用一个树状数组或一个线段树来维护，这样总的复杂度是 $O(nlog^2n)$。
