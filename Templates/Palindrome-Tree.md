【回文树】
## Description
两棵树形成的一个森林，森林中每个结点表示一个回文串，其中一棵树的结点表示的回文串长度为偶数，另一棵树中的结点表示的回文串的长度为奇数；
设一个结点 $v$ 表示的回文串为 $T$，如果存在一个结点 $u$ 和一条标号为 $x$ 边 $v \to u$ ，那么结点 $u$ 表示的串为 $xTx$，当然 $v$ 不为奇数的树的根结点，这个结点比较特殊，它的长度为 $-1$
还存在一个后缀指针，$u \to v$ 表示 $v$ 表示的回文串是 $u$ 表示的回文串的后缀，且是在其它结点中最长的结点。

## Template
```cppp
#include "cstdio"
#include "algorithm"
#include "cstring"
#define maxn 300005
#define sigma 26
typedef long long LL;
const int MAXN = 300005 ;
const int N = 26*2 ;

int f(char c) {
    if ('a' <= c && c <= 'z') return c - 'a';
    else return c - 'A' + 26;
}

struct Palindromic_Tree {
    int next[MAXN][N] ;//next指针，next指针和字典树类似，指向的串为当前串两端加上同一个字符构成
    int fail[MAXN] ;//fail指针，失配后跳转到fail指针指向的节点
    int cnt[MAXN] ;
    int num[MAXN] ;
    int len[MAXN] ;//len[i]表示节点i表示的回文串的长度
    int S[MAXN] ;//存放添加的字符
    int last ;//指向上一个字符所在的节点，方便下一次add
    int n ;//字符数组指针 int p ;//节点指针
    int newnode ( int l ) {//新建节点
        for ( int i = 0 ; i < N ; ++ i )
            next[p][i] = 0 ;
        cnt[p] = 0 ;
        num[p] = 0 ;
        len[p] = l ;
        return p ++ ;
    }
    void init () {//初始化
        p = 0 ;
        newnode ( 0 ) ;
        newnode ( -1 ) ;
        last = 0 ;
        n = 0 ;
        S[n] = -1 ;//开头放一个字符集中没有的字符，减少特判
        fail[0] = 1 ;
    }
    int get_fail ( int x ) {//和KMP一样，失配后找一个尽量最长的
        while ( S[n - len[x] - 1] != S[n] ) x = fail[x] ;
        return x ;
    }
    void add ( int c ) {
        S[++ n] = c ;
        int cur = get_fail ( last ) ;//通过上一个回文串找这个回文串的匹配位置
        if ( !next[cur][c] ) {//如果这个回文串没有出现过，说明出现了一个新的本质不同的回文串
            int now = newnode ( len[cur] + 2 ) ;//新建节点
            fail[now] = next[get_fail ( fail[cur] )][c] ;//和AC自动机一样建立fail指针，以便失配后跳转
            next[cur][c] = now ; num[now] = num[fail[now]] + 1 ;
        }
        last = next[cur][c] ;
        cnt[last] ++ ;
    }
    void count () {
        for ( int i = p - 1 ; i>= 0 ; -- i ) cnt[fail[i]] += cnt[i] ;
		//父亲累加儿子的cnt，因为如果fail[v]=u，则u一定是v的子回文串！
	}
} p, q;

LL ans = 0;
void dfs(int vl, int vr) {
//    ans += p.cnt[vl] * q.cnt[vr];
//    printf("%d %d\n", vl, p.num[vl]);
    for (int i = 0; i < sigma; i++) {
        if (p.next[vl][i] && q.next[vr][i]) {
            ans += (LL)p.cnt[p.next[vl][i]] * q.cnt[q.next[vr][i]];
            dfs(p.next[vl][i], q.next[vr][i]);
        }
    }
}

char s[maxn];
int i,j,k;
int main(){
   // freopen("G.in", "r", stdin);

    int T, cnt = 1;
    scanf("%d", &T);
    while (T --) {
        scanf("%s",s);
        j=strlen(s);
        p.init();
        for(i=0;i<j;++i) p.add(f(s[i]));
        p.count();

        scanf("%s", s);
        j = strlen(s);
        q.init();
        for (int i = 0; i < j; i++) q.add(f(s[i]));
        q.count();

        ans = 0;
        dfs(0, 0);
        dfs(1, 1);
        printf("Case #%d: %lld\n", cnt ++, ans);
    }
    return 0;
}
```
