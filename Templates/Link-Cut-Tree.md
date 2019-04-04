## Description
用链的形式，维护一个森林，可以动态加边，删边；需要注意的是链是没有公共结点的，同时，一个点也可以是一个链。

对于一个splay，对应原树的一条链，splay是以各结点的深度作为关键字排序的，对于splay的顶点，它的parent指针指向这条链最浅的的parent结点。

**动态树的一些操作**

- 【 void access(int v) 】 : 构造出一个splay表示原图中从顶点到 $v$ 的链，同时不改变原有的性质。
    splay(v) 使 $v$ 的左子树表示 $v$ 所在的链中是 $v$ 的祖先结点的集合，这一部是在要求的链中的，而右子树则以单独的链的形式而存在。
    对于这个链的上一个链进行同样的操作，直到不能操作为止。
- 【 void make_root(int v) 】 : 使 $v$ 成为原图中的顶点。
    access(v)
    splay(v)
    翻转 $v$ 结点表示的子树。

- 【 void link(int u, int v) 】 : 加边 $(u, v)$ 。
    make_root(u)
    fa[u] = v
- 【 void cut(int u, int v) 】 : 删除边 $(u, v)$ 。
    make_root(u)
    access(v)
    这样只用删除 $v$ 和 $v$ 的父结点对应的边就可以了
    splay(v)
    fa[ch[v][0]] = 0
    ch[v][0] = 0

- 【 int find(int v) 】: 返回 $v$ 结点所在的链的最浅的结点。
    splay(v)
    再找最左的结点。

## Template
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

struct LCT {
    int ch[N][2];
    int fa[N];
    bool rev[N];
    int root[N];
    int n;

    void init(int _n) {
        n = _n;
        for (int i = 1; i <= n; i++) {
            ch[i][0] = ch[i][1] = 0;
            fa[i] = 0;
            rev[i] = 0;
            root[i] = 1;
        }
    }

    void push_down(int v) {
        if (rev[v]) {
            swap(ch[v][0], ch[v][1]);
            rev[ch[v][0]] ^= 1;
            rev[ch[v][1]] ^= 1;
            rev[v] ^= 1;
        }
    }

    void fly(int v) {
        if (!root[v]) fly(fa[v]);
        push_down(v);
    }

    void rotate(int v, int f) {
        if (root[v]) return ;
        int u = fa[v];
        if (!root[u]) ch[ fa[u] ][ ch[fa[u]][1] == u ] = v;
        fa[v] = fa[u];
        ch[u][f ^ 1] = ch[v][f];
        if (ch[v][f]) fa[ ch[v][f] ] = u;
        ch[v][f] = u; fa[u] = v;
        if (root[u]) root[v] = 1,
        root[u] = 0;
    }

    void splay(int v) {
        fly(v);
        while (!root[v]) {
            int u = fa[v];
            if (root[u]) rotate(v, ch[u][0] == v);
            else {
                int f = ch[u][0] == v;
                if (ch[fa[u]][f ^ 1] == u)
                    rotate(u, f), rotate(v, f);
                else rotate(v, f), rotate(v, f ^ 1);
            }
        }
    }
    void access(int v) { // orz("--------------"); // orz(v); // print();
        for (int rv = 0; v; rv = v, v = fa[v]) {
            splay(v);
            root[ch[v][1]] = true;
            ch[v][1] = rv;
            if (rv && root[rv])
                root[rv] = 0;
        } // print();
    }
    void make_root(int v) {
        access(v);
        splay(v);
        rev[v] ^= 1;
    }
    void link(int u, int v) {
        make_root(u);
        fa[u] = v;
    }

    void cut(int u, int v) {
        make_root(u);
        access(v);
        splay(v);
        fa[ch[v][0]] = 0;
        root[ch[v][0]] = true;
        ch[v][0] = 0;
    }
    int find(int v) {
        splay(v);
        int ret = v;
        while (!ch[ret][0]) ret = ch[ret][0];
    }

    void print() {
        printf("Node\tFa\tLc\tRc\tRev\tRoot\n");
        for (int i = 1; i <= n; i++) {
            fly(i);
            printf("%d\t%d\t%d\t%d\t%d\t%s\n", i, fa[i], ch[i][0], ch[i][1], rev[i], root[i] ? "YES":"NO");
        }
    }
} dst;
int main(void) {
#ifdef owly
    freopen("a.in", "r", stdin);
    //freopen("a.out", "w", stdout);
#endif // owly
    dst.init(8);
    dst.link(1, 2);
    dst.link(2, 7);
    dst.link(7, 8);
    dst.link(3, 4);
    dst.link(4, 5);
    dst.link(4, 6);
    dst.link(3, 2);
    dst.print();
    dst.cut(3, 4); //dst.access(7); orz(dst.find(7));
    dst.print();
    return 0;
}


```
## Problem [SPOJ QTREE - Query on a tree](http://www.spoj.com/problems/QTREE/)
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
const int N = 2e5 + 100;
const int M = 32;

struct LCT {
    int ch[N][2];
    int fa[N];
    bool rev[N];
    bool root[N];
    int n;

    int val[N];
    int max_val[N];

    void push_up(int v) {
        max_val[v] = val[v];
        cmax(max_val[v], max_val[ch[v][0]]);
        cmax(max_val[v], max_val[ch[v][1]]);
    }

    void push_down(int v) {
        if (!rev[v]) return ;
        rev[ch[v][0]] ^= 1;
        rev[ch[v][1]] ^= 1;
        rev[v] ^= 1;
        swap(ch[v][0], ch[v][1]);
    }

    void fly(int v) {
        if (!root[v])
            fly(fa[v]);
        push_down(v);
    }

    void init(int _n) {
        n = _n;
        for (int i = 0; i <= n; i++) {
            ch[i][0] = ch[i][1] = 0;
            fa[i] = 0;
            rev[i] = 0;
            root[i] = 1;
            val[i] = 0;
            max_val[i] = 0;
        }
    }

    void rot(int v, int f) {
        if (root[v]) return ;
        int y = fa[v];
        if (!root[y])
            ch[fa[y]][ ch[fa[y]][1] == y ] = v;
        else
            root[y] = false, root[v] = true;
        fa[v] = fa[y];
        ch[y][f ^ 1] = ch[v][f];
        if (ch[v][f])
            fa[ch[v][f]] = y;
        ch[v][f] = y;
        fa[y] = v;
        push_up(y);
    }

    void splay(int v) {
        fly(v);
        while (!root[v]) {
            int y = fa[v];
            if (root[y])
                rot(v, ch[y][0] == v);
            else {
                int f = ch[fa[y]][0] == y;
                if (ch[y][f] == v)
                    rot(v, f ^ 1), rot(v, f);
                else
                    rot(y, f), rot(v, f);
            }
        }
        push_up(v);
    }

    void access(int v) {
        for (int rv = 0; v; rv = v, v = fa[v]) {
            splay(v);
            if (ch[v][1])
                root[ch[v][1]] = true;
            ch[v][1] = rv;
            if (rv && root[rv])
                root[rv] = false;
            push_up(v);
        }
    }

    void make_root(int v) {
        access(v);
        splay(v);
        rev[v] ^= 1;
    }

    void link(int u, int v) {
        make_root(u);
        fa[u] = v;
    }

    void update_val(int v, int x) {
        make_root(v);
        val[v] = x;
        push_up(v);
    }

    int query(int u, int v) {
        make_root(u);
        access(v);
        splay(u);
        return max_val[u];
    }

    void print() {
        printf("Node\tFa\tLc\tRc\tval\tmax_val\tRev\n");
        for (int i = 0; i <= n; i++) {
            fly(i);
            printf("%d\t%d\t%d\t%d\t%d\t%d\t%d\n", i, fa[i], ch[i][0], ch[i][1], val[i], max_val[i], rev[i]);
        }
    }
} da;



int main(void) {
#ifdef owly
    freopen("data.in", "r", stdin);
#endif // owly

    int T;
    scanf("%d", &T);
    while (T --) {
        int n;
        scanf("%d", &n);

        da.init(2 * n);

        int from, to, cost;
        for (int i = 1; i <= n - 1; i++) {
            scanf("%d%d%d", &from, &to, &cost);
            da.link(from, n + i);
            da.link(to, n + i);
            da.update_val(n + i, cost);
        }

        char cmd[10];
        while (true) {
            scanf("%s", cmd);
            if (cmd[0] == 'D') break;
            if (cmd[0] == 'Q') {
                scanf("%d%d", &from, &to);
                printf("%d\n", da.query(from, to));
                //da.print();
            } else {
                scanf("%d%d", &from, &cost);
                da.update_val(n + from, cost);
            }
        }
    }

    return 0;
}

```
