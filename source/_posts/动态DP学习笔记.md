---
title: 动态DP学习笔记
date: 2022-05-30 20:01:15
categories: 
- [DP, 动态DP]
- [数学, 矩阵]
- [数据结构, 线段树]
- [树, 树链剖分]
tags: "省选/NOI-"
---
# 动态 DP 学习笔记

很神奇的一个算法, 结合了线段树, 矩阵, 树剖等多个算法, 老缝合怪了.

---

## 简介

动态 DP 是一个可以解决带修改的树上问题的利器. 许多小清新的树上问题带上修改的时候就会非常棘手, 使用动态 DP 就可高效地解决了.

另: 本文不讲解全局平衡二叉树, 因为本人也不会.

## 前置知识

线段树, 基本的矩阵知识(最起码会矩阵乘法), 树剖.

## 讲解

由于动态 DP 的特殊性, 我们通过一个模板题来讲解.

### 例

#### 题目:[P4719 【模板】"动态 DP"&动态树分治](https://www.luogu.com.cn/problem/P4719)

#### 题目大意: 

- 给定一棵 $n$ 个节点带点权的树, 第 $i$ 个节点全职为 $a_i$.
- $m$ 次修改, 每次修改将点 $x$ 的权值改为 $w$, 并输出此时的最大独立集.
- $1 \leq n,m \leq 10^5, -10^2 \leq w \leq 10^2$

#### 分析

我们发现, 如果没有修改, 这是一道树形 DP 的板子. 设 $f_{i,0/1}$ 表示 点 $i$ 不选 / 选的最大独立集, 则答案为 $\max(f_{1,0}, f_{1,1})$, 有状态转移方程:

$$
\begin{align}
f_{i,0} &= \sum_{\text{$j$ 为 $i$ 的儿子}}{\max(f_{j,0}, f_{j,1}}) \nonumber \\\\
f_{i,1} &= \sum_{\text{$j$ 为 $i$ 的儿子}}{f_{j, 0} + a_i} \nonumber
\end{align}
$$ 

但题目要求修改, 我们发现每次修改只会对从 $x$ 到根的链上的节点信息产生影响, 所以我们容易想到用树剖来加快修改. 为了能够实现区间的快速修改, 我们需要对状态转移做出改变, 使其可以通过线段树加速修改.

我们定义状态 $g_{i,0/1}$ 表示 $i$ 不选 / 选时, 轻儿子对 $f_{i,0/1}$ 的贡献, 则有转移方程:

$$
\begin{align}
f_{i,0} &= \max{f_{j,0}, f_{j,0}} + g_{i,0}, & \text{$j$ 为重儿子}\nonumber \\\\
f_{i,1} &= f_{j,0} + g_{i,1} + a_i, & \text{$j$ 为重儿子} \nonumber \\\\
g_{i,0} &= \sum{\max(f_{j,0}, f_{j,1})}, & \text{$j$ 为轻儿子} \nonumber \\\\
g_{i,1} &= \sum{f_{j,0}}, & \text{$j$ 为轻儿子} \nonumber
\end{align}
$$

我们发现 $a_i$ 很多余, 于是将其加入 $g_{i,1}$. 则最终的转移方程为:

$$
\begin{align}
f_{i,0} &= \max{f_{j,0}, f_{j,0}} + g_{i,0}, & \text{$j$ 为重儿子}\nonumber \\\\
f_{i,1} &= f_{j,0} + g_{i,1}, & \text{$j$ 为重儿子} \nonumber \\\\
g_{i,0} &= \sum{\max(f_{j,0}, f_{j,1})}, & \text{$j$ 为轻儿子} \nonumber \\\\
g_{i,1} &= \sum{f_{j,0}} + a_i, & \text{$j$ 为轻儿子} \nonumber
\end{align}
$$

我们发现这个转移本质是一种递推, 所以我们想到用矩阵来维护. 考虑一个矩阵序列 $F$, 其中 $F_i$ 为:

$$
\begin{bmatrix}
{f_{i,0}}&{f_{i,1}}
\end{bmatrix}
$$

我们希望其可以通过矩阵乘法来转移, 但我们发现转移方程中有 $\max$ 运算阻止我们进一步转化, 于是我们大胆定义一种矩阵运算 $*$ 满足如下规则:

$$
C_{i,j} = \max(A_{i,k} + B_{k,j})
$$

则有如下转移:

$$
\begin{bmatrix}
{f_{i,0}}&{f_{i,1}}
\end{bmatrix}
=
\begin{bmatrix}
{f_{j,0}}&{f_{j,1}}
\end{bmatrix}
*
\begin{bmatrix}
{g_{i,0}}&{g_{i,1}}\\\\
{g_{i,0}}&-\infty\\\\
\end{bmatrix}
$$

我们发现转移方程中一个数字只与 $f$ 有关, 另一个只与 $g$ 有关, 而且转移符合结合律, 我们可以用线段树快速计算一段区间的 $g$. 至于怎么想到的, 只能说当初发明这个算法的人的天才般的大脑了.

整个算法的精髓就在于此了, 我们可以通过树剖 + 线段树修改一段重链上的矩阵 $*$. 由于转移是由深度由大到小转移的, 所以我们需要将重链的末尾 $tail$ 保存方便使用.

一个小 trick 是结尾的矩阵可以表示为 $\begin{bmatrix} 0&a_i\\\\ 0&-\infty \end{bmatrix} $, 这样就可以将矩阵转移统一为 $g$, 不用区分 $f$ 了.

如果还有不懂的可以看代码.

#### 代码

```C++
#include<iostream>
#include<cstdio>
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
#define mid ((l + r) >> 1)
#define ls (p << 1)
#define rs (p << 1 | 1)
typedef long long ll;
typedef unsigned long long ull;
#define Maxq priority_queue<int,vector<int>,greater<int> >
using namespace std;
int mylog(int a) {
	int ans=0;
	if(a&0xffff0000) {
		ans+=16;
		a>>=16;
	}
	if(a&0x0000ff00) {
		ans+=8;
		a>>=8;
	}
	if(a&0x000000f0) {
		ans+=4;
		a>>=4;
	}
	if(a&0x0000000c) {
		ans+=2;
		a>>=2;
	}
	if(a&0x00000002) {
		ans+=1;
		a>>=1;
	}
	return ans;
}
inline int read() {
	register int a=0,b=0;
	register char c;
	c=getchar();
	while(c<'0'||c>'9') {
		if(c=='-')b=1;
		c=getchar();
	}
	while(c>='0'&&c<='9'){
		a*=10;
		a+=c-'0';
		c=getchar();
	}
	return b?-a:a;
}
const int MAXN = 2e5;
const int INF = 0x3f3f3f3f;
struct Matrix{
	int c[2][2];
	struct Matrix operator * (Matrix b){
		Matrix tmp;
		tmp.c[0][0] = max(c[0][0] + b.c[0][0], c[0][1] + b.c[1][0]);
		tmp.c[0][1] = max(c[0][0] + b.c[0][1], c[0][1] + b.c[1][1]);
		tmp.c[1][0] = max(c[1][0] + b.c[0][0], c[1][1] + b.c[1][0]);
		tmp.c[1][1] = max(c[1][0] + b.c[0][1], c[1][1] + b.c[1][1]);
		return tmp;
	}
};

vector<int> nxt[MAXN];
int a[MAXN], f[MAXN][2];
struct Matrix tree[MAXN], g[MAXN];
int siz[MAXN], dfn[MAXN], id[MAXN], dep[MAXN], top[MAXN], tail[MAXN], son[MAXN], fa[MAXN], end1;
void dfs1(int p, int ff){
	fa[p] = ff;
	siz[p] = 1;
	dep[p] = dep[fa[p]] + 1;
	f[p][1] = a[p];
	int maxx = 0;
	for(auto x : nxt[p]){
		if(x == fa[p]) continue;
		dfs1(x, p);
		siz[p] += siz[x];
		if(siz[x] > maxx){
			maxx = siz[x];
			son[p] = x;
		}
		f[p][1] += f[x][0];
		f[p][0] += max(f[x][0], f[x][1]);
	}
	return;
}
void dfs2(int p){
	dfn[p] = ++end1;
	id[end1] = p;
	if(!top[p]) top[p] = p;
	tail[p] = p;
	g[p].c[0][1] = a[p];
	g[p].c[1][1] = -INF;
	if(son[p]){
		top[son[p]] = top[p];
		dfs2(son[p]);
		tail[p] = tail[son[p]];
	}
	else return;
	for(auto x : nxt[p]){
		if(x == son[p] || x == fa[p]) continue;
		dfs2(x);
		g[p].c[0][0] += max(f[x][0], f[x][1]);
		g[p].c[0][1] += f[x][0];
	}
	g[p].c[1][0] = g[p].c[0][0];
	return;
}
void build(int p, int l, int r){
	if(l == r){
		tree[p] = g[id[l]];
		return;
	}
	build(ls, l, mid);
	build(rs, mid + 1, r);
	tree[p] = tree[rs] * tree[ls];
	return;
}
void modify(int p, int l, int r, int x){
	if(l == r){
		tree[p] = g[x];
		return;
	}
	if(dfn[x] <= mid) modify(ls, l, mid, x);
	else modify(rs, mid + 1, r, x);
	tree[p] = tree[rs] * tree[ls];
	return;
}
struct Matrix query(int p, int l, int r, int x, int y){
	if(x <= l && r <= y) return tree[p];
	if(y <= mid) return query(ls, l, mid, x, y);
	else if(x > mid) return query(rs, mid + 1, r, x, y);
	else return query(rs, mid + 1, r, x, y) * query(ls, l, mid, x, y);
}
int main(){
	int n = read(), m = read();
	rep(i, 1, n) a[i] = read();
	frep(i, 1, n){
		int x = read(), y = read();
		nxt[x].push_back(y);
		nxt[y].push_back(x);
	}
	dfs1(1, 0);
	dfs2(1);
	build(1, 1, n);
	rep(i, 1, m){
		int x = read(), w = read();
		g[x].c[0][1] += w - a[x];
		a[x] = w;
		while(x){
			struct Matrix last, now;
			last = query(1, 1, n, dfn[top[x]], dfn[tail[top[x]]]);
			modify(1, 1, n, x);
			now = query(1, 1, n, dfn[top[x]], dfn[tail[top[x]]]);
			x = fa[top[x]];
			g[x].c[0][1] += now.c[0][0] - last.c[0][0];
			g[x].c[0][0] += max(now.c[0][0], now.c[0][1]) - max(last.c[0][0], last.c[0][1]);
			g[x].c[1][0] = g[x].c[0][0];
		}
		Matrix ans = query(1, 1, n, 1, dfn[tail[1]]);
		printf("%d\n", max(ans.c[0][0], ans.c[0][1]));
	}
	return 0;
}
```