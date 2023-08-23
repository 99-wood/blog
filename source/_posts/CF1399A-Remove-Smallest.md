---
title: CF1399A Remove Smallest
date: 2020-08-06 18:16:06
categories: "入门"
tags: "入门"
---

# CF1399A Remove Smallest

## 题目

### A. Remove Smallest

***time limit per test:1 second***

***memory limit per test:256 megabytes***

***input:standard input***

***output:standard output***

You are given the array a consisting of n positive (greater than zero) integers.

In one move, you can choose two indices i and j (i≠j) such that the absolute difference between ai and aj is no more than one (|ai−aj|≤1) and remove the smallest of these two elements. If two elements are equal, you can remove any of them (but exactly one).

Your task is to find if it is possible to obtain the array consisting of only one element using several (possibly, zero) such moves or not.

You have to answer t independent test cases.

#### Input
The first line of the input contains one integer t (1≤t≤1000) — the number of test cases. Then t test cases follow.

The first line of the test case contains one integer n (1≤n≤50) — the length of a. The second line of the test case contains n integers a1,a2,…,an (1≤ai≤100), where ai is the i-th element of a.

#### Output
For each test case, print the answer: "YES" if it is possible to obtain the array consisting of only one element using several (possibly, zero) moves described in the problem statement, or "NO" otherwise.

#### Example

**input**

5
3
1 2 2
4
5 5 5 5
3
1 2 4
4
1 3 4 4
1
100

**output**

YES
YES
NO
NO
YES

#### Note
In the first test case of the example, we can perform the following sequence of moves:

choose i=1 and j=3 and remove ai (so a becomes [2;2]);
choose i=1 and j=2 and remove aj (so a becomes [2]).
In the second test case of the example, we can choose any possible i and j any move and it doesn't matter which element we remove.

In the third test case of the example, there is no way to get rid of 2 and 4.

## 翻译

[百度翻译结果](https://fanyi.baidu.com/?aldtype=16047#en/zh/A.%20Remove%20Smallest%0Atime%20limit%20per%20test1%20second%0Amemory%20limit%20per%20test256%20megabytes%0Ainputstandard%20input%0Aoutputstandard%20output%0AYou%20are%20given%20the%20array%20a%20consisting%20of%20n%20positive%20(greater%20than%20zero)%20integers.%0A%0AIn%20one%20move%2C%20you%20can%20choose%20two%20indices%20i%20and%20j%20(i%E2%89%A0j)%20such%20that%20the%20absolute%20difference%20between%20ai%20and%20aj%20is%20no%20more%20than%20one%20(%7Cai%E2%88%92aj%7C%E2%89%A41)%20and%20remove%20the%20smallest%20of%20these%20two%20elements.%20If%20two%20elements%20are%20equal%2C%20you%20can%20remove%20any%20of%20them%20(but%20exactly%20one).%0A%0AYour%20task%20is%20to%20find%20if%20it%20is%20possible%20to%20obtain%20the%20array%20consisting%20of%20only%20one%20element%20using%20several%20(possibly%2C%20zero)%20such%20moves%20or%20not.%0A%0AYou%20have%20to%20answer%20t%20independent%20test%20cases.%0A%0AInput%0AThe%20first%20line%20of%20the%20input%20contains%20one%20integer%20t%20(1%E2%89%A4t%E2%89%A41000)%20%E2%80%94%20the%20number%20of%20test%20cases.%20Then%20t%20test%20cases%20follow.%0A%0AThe%20first%20line%20of%20the%20test%20case%20contains%20one%20integer%20n%20(1%E2%89%A4n%E2%89%A450)%20%E2%80%94%20the%20length%20of%20a.%20The%20second%20line%20of%20the%20test%20case%20contains%20n%20integers%20a1%2Ca2%2C%E2%80%A6%2Can%20(1%E2%89%A4ai%E2%89%A4100)%2C%20where%20ai%20is%20the%20i-th%20element%20of%20a.%0A%0AOutput%0AFor%20each%20test%20case%2C%20print%20the%20answer%3A%20%22YES%22%20if%20it%20is%20possible%20to%20obtain%20the%20array%20consisting%20of%20only%20one%20element%20using%20several%20(possibly%2C%20zero)%20moves%20described%20in%20the%20problem%20statement%2C%20or%20%22NO%22%20otherwise.)

## 思路

**自己想**

## 代码
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<string>
#include<cstring>
#include<algorithm>
#include<vector>
#include<queue>
#include<stack>
#include<map>
using namespace std;
int a[100];
int main(){
	int t;
	scanf("%d",&t);
	while(t--){
		int n;
		scanf("%d",&n);
		for(int i=0;i<n;++i){
			scanf("%d",&a[i]);
		}
		sort(a,a+n);
		bool flag=true;
		for(int i=1;i<n;++i){
			if(a[i]-a[i-1]>1){
				flag=false;
				printf("NO\n");
				break;
			}
		}
		if(flag){
			printf("YES\n");
		}
	}
	return 0;
}
//	freopen("data.in","r",stdin);
//	freopen("test.out","w",stdout);
```