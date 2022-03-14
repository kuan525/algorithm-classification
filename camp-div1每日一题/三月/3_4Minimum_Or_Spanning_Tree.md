题面：

>给出n个点， m条边的无向图， 每条边连接 u,v两个端点，边权为 w， 求图的生成树的最小代价。
>
>在这道题中， 我们定义一棵生成树的代价为他所有边的边权按位或得到的值。
>
>#### 输入格式
>
>第一行两个数字 n 和 m , n 表示点数，m 表示图的边数。
>
>接下来 m 行 , 每行三个整数 u,v,w，表示点 u 和点 v 之间存在一条边权为 w 的边。
>
>#### 输出格式
>
>一行， 描述生成树的最小代价。

思路历程：

>*1700
>
>先把代码放一波，晚点儿更新题解：

我的代码：

```c++
#include <bits/stdc++.h>

using namespace std;

const int N = 4e5+10;
int f[N];
int n, m; 
vector<int> bk;//这些位置未为0

struct Edge{
	int a, b, w;
	bool operator<(const Edge &e) const {
        return w < e.w;
    }
}edge[N];

int find(int x){
	if(x != f[x]) f[x] = find(f[x]);
	return f[x];
}

bool check(int w, int u){
	if(w & (1<<u)) return false;
	for(auto t : bk)
		if(w & (1<<t)) return false;
	return true;
}

bool kruskal(int u){//在讨论当前位置能不能是0
	for(int i = 0; i <= n; i ++) f[i] = i;
	sort(edge, edge+m);

	int cnt = 0;
	for(int i = 0; i < m; i ++){
		int a = edge[i].a, b = edge[i].b, w = edge[i].w;
		//需要判断当前这个权值是不是可以为0，以及bk中的所有位置都应该为0
		if(!check(w, u)) continue;//不满足，返回false

		a = find(a); b = find(b);
		if(a != b){
			cnt ++;
			f[a] = f[b];
		}
	}
	if(cnt != n-1) return false;
	return true;
}

int main()
{
	cin >> n >> m;
	for(int i = 0; i < m; i ++)
		scanf("%d%d%d", &edge[i].a, &edge[i].b, &edge[i].w);
	
	for(int i = 29; i >= 0; i --){// 2^i
		//满足bk里面所有位置上面都是0，同时当前位置可以是0
		if(kruskal(i)) bk.push_back(i);
	}
	// cout << bk.size() << endl;
	int ans = (1<<30)-1;
	for(auto t : bk) ans -= (1<<t);
	printf("%d\n", ans);
	return 0;
}
```

