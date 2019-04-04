## 树的编码
对于一棵n节点的树，可以用n-2的长度的序列来唯一表示。即 $prufer编码$。

$prufer 编码$ 对于一棵具有标号的树，不停的找标号最小的叶子结点，并将与该结点相连的结点的编号放入序列中，最后只剩下两个结点时，形成的序列是该树的编码。
>证明$prufer 编码$和树的形态是一一对应的：

这样，一棵n结点的树拥有$n^{n-2}$种结构。在$prufer 编码$中，每一位都有n种可能，所以种类数为$n^{n-2}$

还有一种情况，对于每个结点度数为$v_i$的树，其可能的个数为$\frac {(n-2)!} {v_n! v_{n-1}! \dots v_1!}$

---
## Fabonacci数列性质
* $f(n) = f(n-1) + f(n-2)$
* $f(n+1)f(n+1) - f(n)f(n+2) = (-1)^n$
* $f(n+k+1) = f(n)f(k) + f(n+1)f(k+1)$
* $gcd(f(n), f(n+1)) = 1, gcd(f(n), f(n+2)) = 1$
* $gcd(f(n), f(m)) = f(gcd(n, m))$

## Multinomial theorem
$$
(x_1 + x_2 + \cdots + x_m) ^ n
= \sum_{k_1+k_2+\cdots+k_m=n} {n \choose {k_1, k_2, \cdots, k_m} } \prod_{t=1}^mx_t^{k_t}
$$
其中，${n \choose {k_1, k_2, \cdots, k_m}} = \frac {n!} {k_1!k_2! \cdots k_m!}$.

且是 $m$ 维 Pascal Pyrmid 数。

## 并查集之间的合并
```cpp
int p1 = find(m + 1, j), p2 = find(u, j);  
int p3 = find(m + 1, p2);  
if (p1 != p3) f[m + 1][p1] = p3;
```
并查集中集合的个数为点数 - 有效合并的次数。
