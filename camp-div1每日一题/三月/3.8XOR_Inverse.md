题面：[daimayuan](http://oj.daimayuan.top/course/10/problem/497)

>给你一个有 n 个非负整数组成的数组 a ，你需要选择一个非负整数 x，对数组 a 的每一个 ai 与 x 进行异或后形成新的数组 b，要求 b 数组的逆序对个数最小，如果有多个 x 满足条件，输出最小的 x。
>
>一个逆序对指 b 数组内对于整数 i,j 满足条件 1≤i<j≤n 且 bi>bj 。
>
>### 输入格式
>
>第一行一个正整数 n 。
>
>接下来一行 n 个非负整数 a1,a2,…,an。
>
>### 输出格式
>
>两个数：b 数组的最小逆序对数和此时 x 的最小值。

思路历程：

>晚上又div3，晚点补冲
>
>思路核心：目标x的每一位对最终结果独立，分别讨论即可
>
>

```c++
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

const int N = 3e5 + 10;
int q[N], backup[N], tmp[N];//原数组，备份数组，归并需要的中转数组
int n;//cnt是当前逆序对数量
ll cnt = 0x3f3f3f3f3f3f3f3f, bk;

void merge_sort(int l, int r){//归并排序，重复更新全局变量bk，递归入口bk为0
	if(l >= r) return ;
	int mid = l + (r - l) / 2;
	merge_sort(l, mid); merge_sort(mid+1, r);

	//现在(l, mid) 和 (mid+1, r)都有序，并且计算过
	int i = l, j = mid+1, tt = 1;
	while(i <= mid && j <= r){
		if(backup[i] > backup[j]){
			tmp[tt ++] = backup[j ++];
			bk += (mid - i + 1);
		}
		else tmp[tt ++] = backup[i ++];
	}
	while(i <= mid) tmp[tt ++] = backup[i ++];
	while(j <= r) tmp[tt ++] = backup[j ++];

	//返回至backup数组
	for(int i = 1, j = l; i < tt; i ++, j ++)
		backup[j] = tmp[i];
}

bool check(int bit){// 1 << 30次方对结果没有影响
	//首先我们知道当前的逆序对为cnt
	//我们需要计算改变之后的逆序对数量
	memcpy(backup, q, 4*(n+1));
	bk = 0;
	for(int i = 1; i <= n; i ++) backup[i] ^= (1 << bit);
	merge_sort(1, n);
	// cout << bk << endl;
	//现在得到了取1的逆序对数量为bk,更优则修改

	//第一个点，特判
	if(bit == 30) { cnt = bk; return false; }
	
	if(bk < cnt){
		cnt = bk;
		for(int i = 1; i <= n; i ++) q[i] ^= (1 << bit);
		// cout << q[1] << " " << q[2] << ' ' << q[3] << endl;
		return true;
	}
	// cout << bit << ' ' << "xxx" << endl;
	return false;
}

int main(){
	cin >> n;
	for(int i = 1; i <= n; i ++) scanf("%d", &q[i]);

	int ans = 0;
	for(int bit = 30; bit >= 0; bit --)//讨论每一位
		if(check(bit)) ans += (1 << bit);

	// bk = 0;
	// memcpy(backup, q, 4*(n+1));
	// merge_sort(1, n);
	// cout << bk << endl;
	// cout << backup[1] << ' ' << backup[2] << ' ' << backup[3] << endl; 
	cout << cnt << ' ' << ans << endl;
	return 0;
}
```

