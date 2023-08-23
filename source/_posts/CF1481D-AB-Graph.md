---
title: CF1481D AB Graph
date: 2021-02-06 09:02:07
categories: 
- [构造题]
tags: "普及+/提高"
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# CF1481D AB Graph

## [题目](https://codeforces.com/contest/1481/problem/D)
	
	Your friend Salem is Warawreh's brother and only loves math and geometry problems. He has solved plenty of such problems, but according to Warawreh, in order to graduate from university he has to solve more graph problems. Since Salem is not good with graphs he asked your help with the following problem.
	
	
	You are given a complete directed graph with n vertices without self-loops. In other words, you have n vertices and each pair of vertices u and v (u≠v) has both directed edges (u,v) and (v,u).
	
	Every directed edge of the graph is labeled with a single character: either 'a' or 'b' (edges (u,v) and (v,u) may have different labels).
	
	You are also given an integer m>0. You should find a path of length m such that the string obtained by writing out edges' labels when going along the path is a palindrome. The length of the path is the number of edges in it.
	
	You can visit the same vertex and the same directed edge any number of times.
	
	Input
	The first line contains a single integer t (1≤t≤500) — the number of test cases.
	
	The first line of each test case contains two integers n and m (2≤n≤1000; 1≤m≤105) — the number of vertices in the graph and desirable length of the palindrome.
	
	Each of the next n lines contains n characters. The j-th character of the i-th line describes the character on the edge that is going from node i to node j.
	
	Every character is either 'a' or 'b' if i≠j, or '*' if i=j, since the graph doesn't contain self-loops.
	
	It's guaranteed that the sum of n over test cases doesn't exceed 1000 and the sum of m doesn't exceed 105.
	
	Output
	For each test case, if it is possible to find such path, print "YES" and the path itself as a sequence of m+1 integers: indices of vertices in the path in the appropriate order. If there are several valid paths, print any of them.
	
	Otherwise, (if there is no answer) print "NO".
	
	Example
	inputCopy
	5
	3 1
	*ba
	b*b
	ab*
	3 3
	*ba
	b*b
	ab*
	3 4
	*ba
	b*b
	ab*
	4 6
	*aaa
	b*ba
	ab*a
	bba*
	2 6
	*a
	b*
	outputCopy
	YES
	1 2
	YES
	2 1 3 2
	YES
	1 3 1 3 1
	YES
	1 2 1 3 4 1 4
	NO

## 思路

特判n=2；

n>2:找三个点（比如123点），一个方向上三边只有aab或aaa这种，然后分别讨论m%3==0||==1||==2。

==0：abaaba...

==1: baabaab...

==2: aabaa...

## 代码

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<string>
#include<cstring>
#include<algorithm>
#include<vector>
#include<stack>
#include<queue>
#include<map>
#define ll long long
#define ull unsigned long long
#define rep(i,a,b) for(int i=(a);i<=(b);++i)
#define frep(i,a,b) for(int i=(a);i<(b);++i)
#define drep(i,a,b) for(int i=(a);i>=(b);--i)
#define fdrep(i,a,b) for(int i=(a);i>(b);--i)
using namespace std;
ll read(){
	register ll a=0,b=1;
	register char c;
	c=getchar();
	while(c<'0'||c>'9'){
		if(c=='-')b=-1;
		c=getchar();
	}
	while(c>='0'&&c<='9'){
		a*=10;
		a+=c-'0';
		c=getchar()*b;
	}
	return a*b;
}
#define Qmin priority_queue<int,vector<int> ,less<int> >
#define Qmax priority_queue<int,vector<int> ,greater<int> >
int mylog(int a){
	int ans=0;
	if(a&0xffff0000){ans+=16;a>>=16;}
	if(a&0x0000ff00){ans+=8;a>>=8;}
	if(a&0x000000f0){ans+=4;a>>=4;}
	if(a&0x0000000c){ans+=2;a>>=2;}
	if(a&0x00000002){ans+=1;a>>=1;}
	return ans;
}
const int N=1e3;
int mapp[N+10][N+10];
int main(){
	int t=read();
	while(t--){
		int n=read(),m=read();
		rep(i,1,n){
			rep(j,1,n){
				if(i==j){
					char c;
					scanf("\n%c",&c);
				}
				else{
					char c;
					scanf("\n%c",&c);
					mapp[i][j]=(c=='a'?0:1);	
				}
			}
		}
		if(n==2){
			if(mapp[1][2]==mapp[2][1]){
				printf("YES\n");
				rep(i,1,m+1){
					printf("%d\n",(i&1)?1:2);
				}
			}
			else{
				if(m&1){
					printf("YES\n");
					rep(i,1,m+1){
						printf("%d\n",(i&1)?1:2);
					}
				}
				else{
					printf("NO\n");
				}
			}
		}
		else{
			printf("YES\n");
			int a,b,c;
			if(mapp[1][2]==mapp[2][3]){
				a=2;
				b=3;
				c=1;
			}
			else if(mapp[2][3]==mapp[3][1]){
				a=3;
				b=1;
				c=2;
			}
			else if(mapp[3][1]==mapp[1][2]){
				a=1;
				b=2;
				c=3;
			}
			if(m%3==0){
				while(m>0){
					printf("%d %d %d ",a,b,c);
					m-=3;
				}
				printf("%d\n",a);
			}
			else if(m%3==1){
				m-=1;
				printf("%d ",b);
				while(m>0){
					printf("%d %d %d ",c,a,b);
					m-=3;
				}
				printf("%d\n",c);
			}
			else{
				m-=2;
				printf("%d %d ",c,a);
				while(m>0){
					printf("%d %d %d ",b,c,a);
					m-=3;
				}
				printf("%d \n",b);
			}
		}
	}
	return 0;
}
```