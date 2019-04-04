## Template
```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#define ll long long
#define N 51005
#define inf 1000000000
using namespace std;

int n,m,tot,dfsclk,pnt[N<<3],nxt[N<<3],anc[N],bst[N],fa[N],idom[N],isem[N];
ll ans[N];
int pos[N],id[N];
int read(){
    int x=0; char ch=getchar();
    while (ch<'0' || ch>'9') ch=getchar();
	while (ch>='0' && ch<='9'){
	    x=x*10+ch-'0'; ch=getchar();
	}
	return x;
}
int les(int x,int y){
    return pos[isem[x]]<pos[isem[y]]?x:y;
}
int getanc(int x){
    if (x==anc[x]) return x;
    int t=getanc(anc[x]);
    bst[x]=les(bst[x],bst[anc[x]]);
    return anc[x]=t;
}
struct gragh{
    int fst[N];
    void add(int x,int y){
        pnt[++tot]=y; nxt[tot]=fst[x]; fst[x]=tot;
    }
    void clr(){
        memset(fst,0,sizeof(fst));
    }
} g1,g2,dom;

void dfs(int x){
    pos[x]=++dfsclk;
    id[dfsclk]=x;
    int p;
    for (p=g1.fst[x]; p; p=nxt[p]){
        int y=pnt[p];
        if (!pos[y]){ dfs(y); fa[y]=x; }
    }
}
ll sum(int x){
    if (ans[x]) return ans[x];
    if (x==n) return ans[x]=n;
    else return ans[x]=x+sum(idom[x]);
}

void solve() {
    int i;
    tot=dfsclk=0;
    g1.clr();
    g2.clr();
    dom.clr();
    for (i=1; i<=m; i++){
        int x=read(),y=read(); g1.add(x,y); g2.add(y,x);
    }
    for (i=1; i<=n; i++)
        anc[i]=bst[i]=i;
        memset(idom,0,sizeof(idom));
        memset(isem,0,sizeof(isem));
        memset(pos,0,sizeof(pos));
        memset(id,0,sizeof(id));
        memset(fa,0,sizeof(fa));
        dfs(n);
        pos[0]=inf;
        for (i=dfsclk; i; i--){
        int x=id[i],p;
        if (i>1){
            for (p=g2.fst[x]; p; p=nxt[p]){
                int y=pnt[p]; if (!pos[y]) continue;
                if (pos[y]<pos[x]){
                    if (pos[y]<pos[isem[x]]) isem[x]=y;
                } else{
                    getanc(y); isem[x]=isem[les(x,bst[y])];
                }
            }
            dom.add(isem[x],x);
        }
        for (p=dom.fst[x]; p; p=nxt[p]){
            int y=pnt[p]; getanc(y);
            if (isem[bst[y]]!=x) idom[y]=bst[y];
                else idom[y]=x;
        }
        for (p=g1.fst[x]; p; p=nxt[p])
            if (fa[pnt[p]]==x) anc[pnt[p]]=x;
    }
    for (i=2; i<=dfsclk; i++){
        int x=id[i];
        if (idom[x]!=isem[x]) idom[x]=idom[idom[x]];
    }
    idom[id[1]]=0;
    memset(ans,0,sizeof(ans));
    for (i=1; i<=n; i++){
        printf("%I64d",pos[i]?sum(i):0);
        putchar((i<n)?' ':'\n');
    }
}
int main(){
	while (~scanf("%d%d",&n,&m)){
        solve();
	}
	return 0;
}
```
