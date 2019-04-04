【后缀树】
```cpp
int t1[N], t2[N], c[N];
bool cmp(int *r, int a, int b, int l) {
    return r[a] == r[b] && r[a + l] == r[b + l];
}

void da(int str[], int sa[], int rank[], int height[], int n, int m) {
    n ++;
    int i, j, p, *x = t1, *y = t2;
    for (i = 0; i < m; i++) c[i] = 0;
    for (i = 0; i < n; i++) c[x[i] = str[i]] ++;
    for (i = 1; i < m; i++) c[i] += c[i - 1];
    for (i = n - 1; i >= 0; i--) sa[--c[x[i]]] = i;
    for (j = 1; j <= n; j <<= 1) { p = 0; for (i = n - j; i < n; i++) y[p ++] = i; for (i = 0; i < n; i++) if (sa[i]>= j) y[p ++] = sa[i] - j;

        for (i = 0; i < m; i++) c[i] = 0;
        for (i = 0; i < n; i++) c[x[y[i]]] ++;
        for (i = 1; i < m; i++) c[i] += c[i - 1];
        for (i = n - 1; i >= 0; i--) sa[--c[x[y[i]]]] = y[i];

        swap(x, y);
        p = 1; x[sa[0]] = 0;
        for (i = 1; i < n; i++)
            x[sa[i]] = cmp(y, sa[i - 1], sa[i], j) ? p - 1 : p ++;
        if (p >= n) break;
        m = p;
    }

    int k = 0;
    n -- ;
    for (i = 0; i <= n; i++) rank[sa[i]] = i;
    for (i = 0; i < n; i++) {
        if (k) k--;
        j = sa[rank[i] - 1];
        while (str[i + k] == str[j + k]) k ++;
        height[rank[i]] = k;
    }
}

void printSA(int *r,int *sa,int *rk,int *height,int n)
{
    int i,j;
    cout<<"i\tsuffixArray[i]\t\theight[i]\tsuffix[i]"<<endl;
    for (i=0;i<=n;i++)
    {
        cout<<i<<"\t\t"<<sa[i]<<"\t\t"<<height[i]<<"\t\t";
        for (j=sa[i];r[j]!=0;j++)
        {
            // cout<<r[j]<<" ";
            cout<<(char)r[j]<<" ";
        }
        cout<<endl;
    }
}
```
