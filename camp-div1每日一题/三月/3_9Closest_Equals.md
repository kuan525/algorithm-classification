```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;
const int B=5e5+10;
int read() {int x;scanf("%d",&x);return x;}
int n,m,k;
int f[B][21];
int g[B][21];
int logg[B];
int pre[B];
int b[B];
int a[B];
int q[B];
int p[B];
int cnt; 
int nw[B];
int main()
{
    n=read(),m=read();
    for (int i=1;i<=n;i++)
    {
        a[i]=read();
        b[i]=a[i];
    }
    sort(b+1,b+1+n);
    int len=unique(b+1,b+1+n)-b-1;
    for (int i=1;i<=n;i++)
    {
        a[i]=lower_bound(b+1,b+1+len,a[i])-b;
    }
     
    for (int i=1;i<=n;i++)
    {
        pre[i]=nw[a[i]];
        int last=nw[a[i]];
        nw[a[i]]=i;
        if (cnt && q[cnt]>=last) continue;
        q[++cnt]=last,p[cnt]=i;
    }
    logg[0]=-1;
    for (int i=1;i<=cnt;i++) logg[i]=logg[i>>1]+1,f[i][0]=p[i]-q[i];
    for (int j=1;j<=20;j++)
        for (int i=1;i+(1<<j)-1<=n;i++)
        {
            f[i][j]=min(f[i][j-1],f[i+(1<<(j-1))][j-1]); 
        }   
    while (m--)
    {
        int l=read(),r=read();
        l=lower_bound(q+1,q+1+cnt,l)-q;
        r=upper_bound(p+1,p+1+cnt,r)-p-1;
        if (l>r) printf("-1\n");
        else
        {
            int s=logg[r-l+1];
            int ans=min(f[l][s],f[r-(1<<s)+1][s]);
            printf("%d\n",ans);
        }
    }
} 
```

