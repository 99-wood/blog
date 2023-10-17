---
title: NEUOJ 2383 流行的字符串 题解
date: 2023-10-17 19:34:08
categories:
- [数据结构, 线段树]
- [字符串, KMP]
tags:
---

# NEUOJ 2383 流行的字符串 题解

## 题目

![](/picture/QQ截图20231017193958.png)

## 分析

一眼感觉可以用线段树维护, 难点在于需要存储哪些数据. 

受 KMP 算法的启发, 若某个节点覆盖区间的后缀相同, 则可以存储这个后缀在字符串 $S$ 当中的位置. 此外还要维护当前区间的答案数.

再考虑如何添加字符串. 可以把叫加入的字符串作为线段树修改的参数(实际代码中使用的是队列, 应为数字更好维护). 若当前区间被覆盖, 就暴力逐个加入字符, 然后修改后缀在 $S$ 的位置. 转移可以用 KMP 的方法, 具体来说是预处理出失陪数组, 再预处理出 $to[i][j]$ 表示在第 $i$ 个字符后加入 $j$ 的转移, 具体可以参考代码.

再考虑如何下传标记. 若未传递字符串长度大于等于 $|S|$, 则直接修改即可. 否则暴力加字符, 若子节点区间的后缀相同, 可以即时修改后缀在 $S$ 的位置.

再考虑如何询问. 基本是正常的线段树操作, 一点不同在于若当前区间答案未知需要分别询问两个子节点, 复杂度可能会受影响, 但是调用层数不会很高(大概). 但是这题时限很宽, 所以还是可以过的.

## 代码

```cpp
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
	while(c>='0'&&c<='9') {
		a*=10;
		a+=c-'0';
		c=getchar();
	}
	return b?-a:a;
}
const int MAXN = 1e5 + 10;
int to[30][30], fail[MAXN];
char str[30];
int len;
struct node{
	queue<int> q;
	int now, cnt;
}tree[MAXN << 2];
void build(int p, int l, int r){
	tree[p].cnt = 0;
	tree[p].now = -1;
	if(l == r) return;
	build(ls, l, mid);
	build(rs, mid + 1, r);
	return;
}
void pushdown(int p, int l, int r){
	if(tree[p].now != -1){
		tree[ls].now = tree[rs].now = tree[p].now;
		while(!tree[p].q.empty()){
			if(tree[ls].q.size() >= len) tree[ls].q.pop();
			if(tree[rs].q.size() >= len) tree[rs].q.pop();
			int x = tree[p].q.front();
			tree[p].q.pop();
			tree[ls].q.push(x);
			tree[rs].q.push(x);
		}
	}
	else{
		while(!tree[p].q.empty()){
			if(tree[ls].q.size() >= len) tree[ls].q.pop();
			if(tree[rs].q.size() >= len) tree[rs].q.pop();
			int x = tree[p].q.front();
			tree[p].q.pop();
			tree[ls].q.push(x);
			if(tree[ls].now != -1) tree[ls].now = to[tree[ls].now][x];
			tree[rs].q.push(x);
			if(tree[rs].now != -1) tree[rs].now = to[tree[rs].now][x];
		}
		if(tree[ls].q.size() >= len && tree[ls].now == -1){
			tree[ls].now = 0;
			rep(i, 1, len){
				int x = tree[ls].q.front();
				tree[ls].q.pop();
				tree[ls].q.push(x);
				tree[ls].now = to[tree[ls].now][x];
			}
		}
		if(tree[rs].q.size() >= len && tree[rs].now == -1){
			tree[rs].now = 0;
			rep(i, 1, len){
				int x = tree[rs].q.front();
				tree[rs].q.pop();
				tree[rs].q.push(x);
				tree[rs].now = to[tree[rs].now][x];
			}
		}
	}
	if(tree[ls].now == len) tree[ls].cnt = (mid - l + 1);
	else if(tree[ls].now != -1) tree[ls].cnt = 0;
	else tree[ls].cnt = -1;
	if(tree[rs].now == len) tree[rs].cnt = (r - mid);
	else if(tree[rs].now != -1) tree[rs].cnt = 0;
	else tree[rs].cnt = -1;
	return;
}
void pushup(int p, int l, int r){
	if(tree[ls].now == tree[rs].now) tree[p].now = tree[ls].now;
	else tree[p].now = -1;
	tree[p].cnt = (tree[ls].cnt < 0 || tree[rs].cnt < 0) ? -1 :tree[ls].cnt + tree[rs].cnt;
	return;
}
void add(int p, int l, int r, int x, int y, queue<int> s){
	if(x <= l && r <= y){
		if(tree[p].now != -1){
			while(!s.empty()){
				if(tree[p].q.size() >= len) tree[p].q.pop();
				int x = s.front();
				s.pop();
				tree[p].q.push(x);
				tree[p].now = to[tree[p].now][x];
			}
		}
		else{
			while(!s.empty()){
				if(tree[p].q.size() >= len) tree[p].q.pop();
				int x = s.front();
				s.pop();
				tree[p].q.push(x);
			}
			if(tree[p].q.size() >= len){
				tree[p].now = 0;
				rep(i, 1, len){
					int x = tree[p].q.front();
					tree[p].q.pop();
					tree[p].q.push(x);
					tree[p].now = to[tree[p].now][x];
				}
			}
		}
		if(tree[p].now == len) tree[p].cnt = (r - l + 1);
		else if(tree[p].now != -1) tree[p].cnt = 0;
		else tree[p].cnt = -1;
		return;
	}
	if(y < l || x > r) return;
	pushdown(p, l, r);
	add(ls, l, mid, x, y, s);
	add(rs, mid + 1, r, x, y, s);
	pushup(p, l, r);
	return;
}
int query(int p, int l, int r, int x, int y){
	if(x <= l && r <= y && tree[p].cnt >= 0) return tree[p].cnt;
	if(y < l || x > r) return 0;
	pushdown(p, l, r);
	int res = query(ls, l, mid, x, y) + query(rs, mid + 1, r, x, y);
	pushup(p, l, r);
	return res;
}
int main(){
	scanf("%s", str + 1);
	len = strlen(str + 1);
	fail[0] = -1;
	rep(i, 1, len){
		for(int j = fail[i - 1]; j != -1; j = fail[j]){
			if(str[j + 1] == str[i]){
				fail[i] = j + 1;
				break;
			}
		}
	}
	rep(i, 1, len){
		rep(c, 'a', 'z'){
			if(c == str[i]){
				to[i - 1][c - 'a'] = i;
			}
			else{
				to[i - 1][c - 'a'] = to[fail[i - 1]][c - 'a'];
			}
		}
	}
	rep(c, 'a', 'z'){
		to[len][c - 'a'] = to[fail[len]][c - 'a'];
	}
	int n = read(), q = read();
	while(q--){
		int op = read();
		if(op == 1){
			int x = read(), y = read();
			char str[30];
			queue<int> q; 
			scanf("%s", str);
			int l = strlen(str);
			frep(i, 0, l){
				q.push(str[i] - 'a');
			}
			add(1, 1, n, x, y, q);
		}
		else{
			int x = read(), y = read();
			int ans = query(1, 1, n, x, y);
			printf("%d\n", ans);
		}
	}
	return 0;
}
```

## 总结

这题细节比较多, 写起来比较头大. 大家看个乐呵就好. 想提交可能也比较困难, NEUOJ 常年不开放注册. 可以和我的代码对拍.