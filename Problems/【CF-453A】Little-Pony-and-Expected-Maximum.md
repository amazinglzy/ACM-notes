[题目链接](http://codeforces.com/problemset/problem/453/A)
【组合数学】

## 题目大意
$1, 2, \dots, m$ 这 $m$ 个数，每个数都有 $\frac {1} {m}$ 可能性选择到，选 $n$ 次，求期望。

## 思路
$$ \frac { \sum_{i = 1}^m \sum_{j=1}^n iC_n^j(i-1)^{n-j} } {m^n}$$
$$ \frac {\sum_{i=1}^m i(i^n -  (i - 1)^n)} {n^m} $$
$$ \frac {\sum_{i=1}^m i^{n+1} - (i-1)^{n+1} - (i-1)^{n+1} } {m^n}$$
$$ m - \frac {\sum_{i = 1}^m (i - 1)^{n + 1} } {m^n}$$


## 代码实现
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>
#include <set>
#include <map>
#include <queue>

#define minv(x,y) ((x) < (y) ? (x) : (y))
#define maxv(x,y) ((x) > (y) ? (x) : (y))
using namespace std;
typedef double db;
typedef long long ll;
typedef unsigned int ui;

int main(void) {
//    freopen("in.txt", "r", stdin);

    int m, n;
	scanf("%d%d", &m, &n);

    db tot = 0;
    for (int i = 0; i < m; i++) {
    	tot += pow((db)i/m, n);
	}
	printf("%f\n", m - tot);


	return 0;
}
```
