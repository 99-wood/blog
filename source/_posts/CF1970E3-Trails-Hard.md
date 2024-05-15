---
title: CF1970E3-Trails (Hard)
date: 2024-05-15 21:43:30
mathjax: true
categories: 
- [数学, 矩阵]
- [数学, 组合数学]
---
# CF1970E3 Trails (Hard) 题解

## [题目](https://codeforces.com/problemset/problem/1970/E3)

题目大意: 给定 $m$ 个点. 每个点都和一个共同的中心点有若干条路径. 对于第 $i$ 个点, 有 $s_i$ 条短路径, $l_i$ 条长路径. 一个小人开始在 $1$ 号点, 每天他要走两条路径到达一个点(可以和出发点相同), 要求每天走过的路径至少有一条长路径. 问走 $n$ 天后, 所有合法路径的方案数.

$1 \leq m \leq 10^5, 1 \leq n \leq 10^9$

## 思路

一个朴素的想法是递推.

设第 $i$ 天在 $j$ 号点的方案数为 $a_{i,j}$.

则有

$$
\begin{aligned}
a_{i, j} &= \sum_{k = 1}^{m}{(s_j s_k + l_j s_k + s_j l_k) \cdot a_{i - 1, k}}
\end{aligned}
$$

令 $w_{i, j} = s_i s_j + l_i s_j + s_i l_j$ 也就是

$$

\begin{bmatrix}
{a_{i,1}}\\\\
{a_{i,2}}\\\\
\vdots\\\\
{a_{i, m}}
\end{bmatrix}

=

\begin{bmatrix}
{w_{1, 1}} & w_{1,2} & \cdots & w_{1, m} \\\\
{w_{2, 1}} & w_{2,2} & \cdots & w_{2, m} \\\\
\vdots & \vdots & \ddots & \vdots \\\\
{w_{m, 1}} & w_{m,2} & \cdots & w_{m, m} \\\\
\end{bmatrix}

\begin{bmatrix}
{a_{i - 1,1}}\\\\
{a_{i - 1,2}}\\\\
\vdots\\\\
{a_{i - 1, m}}
\end{bmatrix}

$$

使用矩阵快速幂可以解决.

但是我们发现 $1 \leq m \leq 10^5$ 我们甚至连一次矩阵乘法都做不完!

我们发现 
$$
\begin{align}
w_{i, j} &= s_i s_j + l_i s_j + s_i l_j \nonumber \\\\
&= (s_i + l_i) \cdot (s_j + l_j) - l_i \cdot l_j \nonumber
\end{align}
$$

这何尝不是一种矩阵乘法?

于是我们构造矩阵

$$
A = 
\begin{bmatrix}
{s_1 + l_1} & l_1\\\\
{s_2 + l_2} & l_2\\\\
\vdots & \vdots\\\\
{s_n + l_n} & l_n\\\\
\end{bmatrix}
$$

$$
B = 
\begin{bmatrix}
{s_1 + l_1} & {s_2 + l_2} & \cdots & {m_2 + l_m}\\\\
-l_1 & -l_2 & \cdots & -l_m\\\\
\end{bmatrix}
$$

那么

$$
\begin{aligned}

\begin{bmatrix}

{a_{n,1}}\\\\
{a_{n,2}}\\\\
\vdots\\\\
{a_{n, m}}

\end{bmatrix}

&=(A B)^n

\begin{bmatrix}

{a_{0, 1}}\\\\
{a_{0, 2}}\\\\
\vdots\\\\
{a_{0, m}}

\end{bmatrix}\\\\

&=
A (B A)^{n - 1} B

\begin{bmatrix}
{a_{0, 1}}\\\\
{a_{0, 2}}\\\\
\vdots\\\\
{a_{0, m}}
\end{bmatrix}

\end{aligned}
$$

而 $BA$ 是一个二阶矩阵, 可以计算!

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
int mylog(int a){
	int ans=0;
	if(a&0xffff0000){
		ans+=16;
		a>>=16;
	}
	if(a&0x0000ff00){
		ans+=8;
		a>>=8;
	}
	if(a&0x000000f0){
		ans+=4;
		a>>=4;
	}
	if(a&0x0000000c){
		ans+=2;
		a>>=2;
	}
	if(a&0x00000002){
		ans+=1;
		a>>=1;
	}
	return ans;
}
inline int read(){
	register int a=0,b=0;
	register char c;
	c=getchar();
	while(c<'0'||c>'9'){
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
const int MAXM = 100010;
const int MOD = 1e9 + 7;
struct Matrix{
	int n, m;
	int **data;
	Matrix(int e) : n(e), m(e){
		data = new int*[n + 1];
		rep(i, 0, n){
			data[i] = new int[m + 1];
		}
		rep(i, 1, n){
			data[i][i] = 1;
		}
	}
	Matrix(int n, int m) : n(n), m(m){
		data = new int*[n + 1];
		rep(i, 0, n){
			data[i] = new int[m + 1];
		}
	}
	Matrix(const Matrix & other) : n(other.n), m(other.m){
		data = new int*[n + 1];
		rep(i, 0, n){
			data[i] = new int[m + 1];
		}
		rep(i, 1, n){
			rep(j, 1, m){
				data[i][j] = other.data[i][j];
			}
		}
	}
	~Matrix(){
		rep(i, 0, n){
			delete[] data[i];
		}
		delete[] data;
	}
	Matrix& operator= (const Matrix & other){
		if(&other == this) return *this;
		rep(i, 0, n){
			delete[] data[i];
		}
		delete[] data;
		n = other.n;
		m = other.m;
		data = new int*[n + 1];
		rep(i, 0, n){
			data[i] = new int[m + 1];
		}
		rep(i, 1, n){
			rep(j, 1, m){
				data[i][j] = other.data[i][j];
			}
		}
		return *this;
	}
	Matrix operator* (const Matrix &other){
		Matrix ans(n, other.m);
//		ans.n = n;
//		ans.m = other.m;
		rep(i, 1, ans.n){
			rep(j, 1, ans.m){
				ans.data[i][j] = 0;
				rep(k, 1, m){
					ans.data[i][j] = ((ll)ans.data[i][j] + (ll)data[i][k] * other.data[k][j] % MOD) % MOD;
					if(ans.data[i][j] < 0){
						cout << 1;
					}
				}
			}
		}
		return ans;
	}
};
Matrix ksm(Matrix a, int k){
	if(k == 0) return Matrix(a.n);
	if(k == 1) return a;
	if(k & 1){
		Matrix tmp = ksm(a, k >> 1);
		return tmp * tmp * a;
	}
	else{
		Matrix tmp = ksm(a, k >> 1);
		return tmp * tmp;
	}
}
int s[MAXM], l[MAXM];
signed main(){
	int m = read(), n = read();
	rep(i, 1, m) s[i] = read();
	rep(i, 1, m) l[i] = read();
	Matrix A(m, 2), B(2, m);
	rep(i, 1, m){
		A.data[i][1] = s[i] + l[i];
		A.data[i][2] = l[i];
	}
	rep(i, 1, m){
		B.data[1][i] = s[i] + l[i];
		B.data[2][i] = MOD - l[i];
	}
	Matrix a0(m, 1);
	rep(i, 2, m) a0.data[i][1] = 0;
	a0.data[1][1] = 1;
	Matrix tmp = ksm(B * A, n - 1) * B * a0;
	Matrix ans = A * tmp;
	ll sum = 0;
	rep(i, 1, m) sum += ans.data[i][1];
	printf("%lld", sum % MOD);
    return 0;
}
```