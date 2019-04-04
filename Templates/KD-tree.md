# KD tree
## Description
## Template
```cpp
namespace KDTree{
	const int maxn=5e4+7;
	const int maxD=6;
	int D; //D为维数
	int n,idx;//n为点数

	struct kdnode{
		int x[maxD];
		int splitD, l, r, fa;
		void set(int s, int L, int R, int f){
			splitD = s; l = L; r = R; fa = f;
		}
		//协助查询
		ll dis;
		bool operator < (const kdnode& rhs) const{
			return dis < rhs.dis;
		}
	}kdtree[maxn];

	int curD = -1; //当前用来作为基准的维数
	bool cmp(kdnode a, kdnode b){
		return a.x[curD] < b.x[curD];
	}

	void cal(int l, int r){
		//计算[l,r]区间用来作为分割基准的值
		//可以计算方差或者直接轮流赋值
		//2.轮流赋值
		curD++;
		curD%=D;
		return;
		//1.计算方差
		static double avg[maxD], var[maxD];
		for(int i=0; i<D; i++)
		    avg[i]=0, var[i]=0;
		for(int i=l; i<=r; i++)
		    for(int j=0; j<D; j++)
		        avg[j]+=1.0*kdtree[i].x[j]/(r-l+1);
		for(int i=l; i<=r; i++)
		    for(int j=0; j<D; j++)
		        var[j]+=1.0*(kdtree[i].x[j]-avg[j])/n*(kdtree[i].x[j]-avg[j]);
		int maxVar=-1.0; for(int i=0; i<D; i++) if(var[i]>maxVar)
		maxVar=var[i],curD=i;
	}

	int build(int fa=-1, int l=0, int r=n-1){ //[l,r]双闭区间
		if(r<l)return -1; int root=(l+r)>>1;
		cal(l, r);
		//sort(kdtree+l, kdtree+r+1, cmp);
		nth_element(kdtree+l, kdtree+root, kdtree+r+1, cmp);
		int d = curD;
		int lson = build(root, l, root-1);
		int rson = build(root, root+1, r);
		kdtree[root].set(d, lson, rson, fa);
		return root;
	}

	ll dist(kdnode a, kdnode b){
		ll res = 0;
		for (int i = 0; i < D; i += 1)
			res +=1ll*(a.x[i]-b.x[i])*(a.x[i]-b.x[i]);
		return res;
	}

	priority_queue<kdnode> kdQ;
	void query(int root, kdnode queryP, int m){
	//查询离queryP最近的m个点
		if(root == -1)return;
		int d = kdtree[root].splitD;
		kdnode tmp = kdtree[root];
		tmp.dis = dist(tmp, queryP);

		int xx = kdtree[root].l, yy = kdtree[root].r;
		ll ds = queryP.x[d] - kdtree[root].x[d];

		if(ds > 0) swap(xx, yy);
		if(xx!=-1)
			query(xx, queryP, m);

		bool flag = false;
		if(sz(kdQ)<m)kdQ.push(tmp), flag = true;
		else{
			if(tmp.dis < kdQ.top().dis) kdQ.pop(), kdQ.push(tmp);

			//如果一维的坐标差方都比最大距离大,那么显然不需要去另外一个儿子节点
			if(ds * ds < kdQ.top().dis) flag = true;
		}
		if(yy!=-1&&flag) query(yy, queryP, m);
	}
};
```
## Problems
