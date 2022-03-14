``` cpp
#include<bits/stdc++.h>
#define maxn 1000005
#define inf 2e9
#define pb push_back
#define rep(i,a,b) for(int i=a;i<=b;i++)
using namespace std;

inline int read()
{
	int x=0,w=1; char c=getchar();
	while(c<'0'||c>'9') {if(c=='-') w=-1; c=getchar();}
	while(c<='9'&&c>='0') x=(x<<1)+(x<<3)+c-'0',c=getchar();
	return w==1?x:-x;
}

inline void write(int x)
{
	if(x>=10) write(x/10);
	putchar(x%10+'0');
}

struct node{int to,w;};
vector <node> mp[maxn];

inline bool operator < (node a,node b){return a.w>b.w;}
inline bool operator > (node a,node b){return a.w<b.w;}
priority_queue <node> q;

int n,m,t,r,k,A[maxn],d[25][maxn],id[maxn],dp[maxn][25],ct[maxn];
bool vis[25][maxn];
double DP[maxn][25],P[maxn];

inline void dij(int id,int uu)
{
	rep(i,1,n) d[id][i]=inf; q.push({uu,0}); d[id][uu]=0;
	while(!q.empty())
	{
		node u=q.top(); q.pop();
		if(vis[id][u.to]) continue; d[id][u.to]=u.w; vis[id][u.to]=1;
		for(auto v:mp[u.to])
		{
			if(d[id][v.to]>d[id][u.to]+v.w)
			{
				d[id][v.to]=d[id][u.to]+v.w;
				q.push({v.to,d[id][v.to]});
			}
		}
	}
}

inline double DFS(int sta,int u)
{
	if(DP[sta][u]) return DP[sta][u];
	double tmp=1.0*P[u]*d[u][n]/t+(1-P[u])*d[u][n]/r;
	rep(i,1,k)
	{
		if(sta&(1<<(i-1))) continue;
		tmp=min(tmp,1.0*(1-P[u])*d[u][n]/r+P[u]*(1.0*d[u][A[i]]/t+DFS(sta|(1<<(i-1)),i)));
	}
	return DP[sta][u]=tmp;
}

int main()
{
	t=read(); r=read(); n=read(); m=read();
	rep(i,1,m)
	{
		int u=read(),v=read(),w=read();
		mp[u].pb({v,w}); mp[v].pb({u,w});
	}
	k=read();
	rep(i,1,k) A[i]=read(),P[i]=read()/100.0,dij(i,A[i]),id[A[i]]=i;
	dij(19,1); dij(20,n); P[19]=1;
	if(d[19][n]==inf) {puts("-1"); exit(0);}
	DFS(0,19); int ed=(1<<k)-1;
	printf("%.6f\n",DP[0][19]);
	return 0;
}

```

