---
title: CCPC2022威海站 B题 Recruitment 题解
date: 2024-07-27 21:27:49
mathjax: true
tags:
categories: 
- [搜索]
- [数学]
---
# CCPC2022威海站 B题 Recruitment 题解

## 题目大意

[原题链接](https://codeforces.com/gym/104023/problem/B)

初始有一个由 $n$ 个**正整数**组成的形如 $a_1 + a_2 + \cdots + a_n$ 的式子, $n - 1$ 个操作每次将其中一个加号改为乘号. 现给出初始的式子和每次修改后的式子的值 $s_i$, 求原来的 $n$ 个正整数以及每次操作的符号的位置. 输出任意方法即可, 无解输出 $-1$.

数据范围： $n\leq10^5, s_i \leq 10^9$.

## 提示

{% spoiler Hint 1 %}
$s_n$ 为所有数的积
{% endspoiler %}

{% spoiler Hint 2 %}
可以根据 $s_n$ 和 $s_{n - 1}$ 求出最后一次修改的加号两侧的数的积
{% endspoiler %}

{% spoiler Hint 3 %}
上面的 Hint2 可以接着推到 $n - 1, n - 2, \cdots$
{% endspoiler %}

{% spoiler Hint 4 %}
若 $s_{i - 1} = s_i + 1$ 说明什么?
{% endspoiler %}

## 解法

如果我们将一次修改理解为将加号两侧的数合并, 合并的数为原来两数的乘积, 就会发现问题实际是求一个可重集使得每次合并后集合中数的和等于 $s_i$. 显然最后集合中只剩下 $s_n$. 则最后一次合并前的两个数设为 $a,b$ 满足 $a + b = s_{n - 1}, ab = s_n$, 可以解方程求出这两数, 把这两个数放入集合. 考虑倒推, 我们每次枚举集合中的数尝试还原, 若成功则接着去找下一个要还原的数, 直到全部还原. 若当前集合中每个数都不能被还原, 说明我们当前的集合是错误的, 需要回退. 这一过程可以搜索解决. 但是 $n \leq 10^5$, 直接搜索复杂度很高. 注意到若 $s_{i - 1} = s_i + 1$ 说明原来的两个数至少有一个是 $1$. 我们将 $s$ 中满足上述条件的剔除, 则剩下的操作前的两个数至少有一个大于等于 $2$, 搜索层数会降到 $\log_2{s_n} = 30$ 层. 实际根本没有这么高.

还可以继续剪枝, 若当前用集合中某个数 $a_i$ 分解后失败, 则当前层不需要再尝试用别的与 $a_i$ 相等的值去分解, 可以用 std::map 记录是否还原过某个数剪枝.

## 代码

```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<string>
#include<cstring>
#include<algorithm>
#include<stack>
#include<vector>
#include<queue>
#include<map>
#include<set>
#include<bitset>
#define rep(i,a,b) for(int i=a;i<=(b);++i)
#define frep(i,a,b) for(int i=a;i<(b);++i)
#define drep(i,a,b) for(int i=a;i>=(b);--i)
#define ls tree[p].l
#define rs tree[p].r
#define mid ((l + r) >> 1)
typedef long long ll;
typedef unsigned long long ull;
#define Maxq priority_queue<int,vector<int>,greater<int> >
using namespace std;
const int MAXN = 1000010;
ll js(ll m, ll n){ //解方程 a * b = m, a + b = n
	if(n * n - 4 * m < 0) return -1;
	ll ans = (n + sqrt(n * n - 4 * m)) / 2;
	if(ans > 0 && n - ans > 0 && ans * (n - ans) == m) return ans;
	else return -1;
}
int n;
ll q[MAXN];
struct node{
	int w, t, l, r, siz;
}tree[MAXN]; //用二叉树记录每个数的还原
int endd;
int add(int w){
	tree[endd].w = w;
	tree[endd].t = -1;
	tree[endd].l = tree[endd].r = -1;
	++endd;
	return endd - 1;
}
void del(){
	--endd;
}
int sum;
int pre[MAXN]; //下一个要还原的操作位置
int id[MAXN]; //id[x]是x处操作再所有没有1的操作中的位置
int rt;
bool dfs(int p, int pos, map<int, bool> &mp){
	if(p == 1){
		return true;
	}
	if(tree[pos].l == -1){
		if(mp[tree[pos].w]) return false;
		mp[tree[pos].w] = true;
		int a = js(tree[pos].w, q[p - 1] - (sum - tree[pos].w));
		if(a != -1){
			tree[pos].t = id[p];
			tree[pos].l = add(a);
			tree[pos].r = add(tree[pos].w / a);
			int tmp = sum;
			sum -= tree[pos].w;
			sum += a + tree[pos].w / a;
			map<int, bool> mpp;
			if(dfs(pre[p], rt, mpp)) return true;
			else{
				del(); del();
				tree[pos].t = -1;
				tree[pos].l = tree[pos].r = -1;
				sum = tmp;
				return false;
			}
		}
		return false;
	}
	else{
		if(dfs(p, tree[pos].l, mp)) return true;
		else if(dfs(p, tree[pos].r, mp)) return true;
		else return false;
	}
}
void print1(int p){
	if(tree[p].t == -1){
		tree[p].siz = 1;
		cout << tree[p].w << " ";
		return;
	}
	print1(tree[p].l);
	print1(tree[p].r);
	tree[p].siz = tree[tree[p].l].siz + tree[tree[p].r].siz;
	return;
}
int out[MAXN];
void print2(int p, int len){
	if(tree[p].t == -1){
		return;
	}
	out[tree[p].t] = len + tree[tree[p].l].siz;
	print2(tree[p].l, len);
	print2(tree[p].r, len + tree[tree[p].l].siz);
	return;
}
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	cin >> n;
	rep(i, 1, n) cin >> q[i];
	int p = 1;
	int cnt = 0;
	pre[1] = -1;

    //去掉1
	rep(i, 2, n){
		if(q[i] - q[i - 1] == -1){
			++cnt;
		}
		else{
			pre[i] = p;
			id[i] = id[p] + 1;
			p = i;
		}
	}
	int cnt_ = cnt;
    //剔除1后更新
	rep(i, 2, n){
		q[i - 1] -= cnt_;
		if(pre[i]) continue;
		else cnt_--;
	}
	bool flag = true;
	rep(i, 2, n){
		if(q[i] <= 0){
			cout << -1;
			return 0;
		}
	}
	rt = add(q[p]);
	sum = q[p];
	map<int, bool> mp;
	if(dfs(p, rt, mp)){
		print1(rt);
		rep(i, 1, cnt) cout << 1 << " ";
		cout << endl;
		print2(rt, 0);
		int last = n - 1;
		rep(i, 2, n){
			if(pre[i]){
				cout << out[id[i]] << endl;
			}
			else cout << (last--) << endl;
		}
	}
	else{
		cout << -1;
	}
	
    return 0;
}
```