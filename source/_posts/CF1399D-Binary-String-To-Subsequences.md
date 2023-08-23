---
title: CF1399D Binary String To Subsequences
date: 2020-08-06 18:17:11
categories: 
- [数据结构,队列]
tags: "普及+/提高"
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# CF1399D Binary-String-To-Subsequences

## 题目

	D. Binary String To Subsequences
	time limit per test2 seconds
	memory limit per test256 megabytes
	inputstandard input
	outputstandard output
	You are given a binary string s consisting of n zeros and ones.
	
	Your task is to divide the given string into the minimum number of subsequences in such a way that each character of the string belongs to exactly one subsequence and each subsequence looks like "010101 ..." or "101010 ..." (i.e. the subsequence should not contain two adjacent zeros or ones).
	
	Recall that a subsequence is a sequence that can be derived from the given sequence by deleting zero or more elements without changing the order of the remaining elements. For example, subsequences of "1011101" are "0", "1", "11111", "0111", "101", "1001", but not "000", "101010" and "11100".
	
	You have to answer t independent test cases.
	
	Input
	The first line of the input contains one integer t (1≤t≤2⋅104) — the number of test cases. Then t test cases follow.
	
	The first line of the test case contains one integer n (1≤n≤2⋅105) — the length of s. The second line of the test case contains n characters '0' and '1' — the string s.
	
	It is guaranteed that the sum of n does not exceed 2⋅105 (∑n≤2⋅105).
	
	Output
	For each test case, print the answer: in the first line print one integer k (1≤k≤n) — the minimum number of subsequences you can divide the string s to. In the second line print n integers a1,a2,…,an (1≤ai≤k), where ai is the number of subsequence the i-th character of s belongs to.
	
	If there are several answers, you can print any.
	
	Example
	inputCopy
	4
	4
	0011
	6
	111111
	5
	10101
	8
	01010000
	outputCopy
	2
	1 2 2 1 
	6
	1 2 3 4 5 6 
	1
	1 1 1 1 1 
	4
	1 1 1 1 1 2 3 4 

## 翻译

[百度翻译结果](https://fanyi.baidu.com/?aldtype=16047#en/zh/D.%20Binary%20String%20To%20Subsequences%0Atime%20limit%20per%20test2%20seconds%0Amemory%20limit%20per%20test256%20megabytes%0Ainputstandard%20input%0Aoutputstandard%20output%0AYou%20are%20given%20a%20binary%20string%20s%20consisting%20of%20n%20zeros%20and%20ones.%0A%0AYour%20task%20is%20to%20divide%20the%20given%20string%20into%20the%20minimum%20number%20of%20subsequences%20in%20such%20a%20way%20that%20each%20character%20of%20the%20string%20belongs%20to%20exactly%20one%20subsequence%20and%20each%20subsequence%20looks%20like%20%22010101%20...%22%20or%20%22101010%20...%22%20(i.e.%20the%20subsequence%20should%20not%20contain%20two%20adjacent%20zeros%20or%20ones).%0A%0ARecall%20that%20a%20subsequence%20is%20a%20sequence%20that%20can%20be%20derived%20from%20the%20given%20sequence%20by%20deleting%20zero%20or%20more%20elements%20without%20changing%20the%20order%20of%20the%20remaining%20elements.%20For%20example%2C%20subsequences%20of%20%221011101%22%20are%20%220%22%2C%20%221%22%2C%20%2211111%22%2C%20%220111%22%2C%20%22101%22%2C%20%221001%22%2C%20but%20not%20%22000%22%2C%20%22101010%22%20and%20%2211100%22.%0A%0AYou%20have%20to%20answer%20t%20independent%20test%20cases.)
[百度翻译结果](https://fanyi.baidu.com/?aldtype=16047#en/zh/Input%0AThe%20first%20line%20of%20the%20input%20contains%20one%20integer%20t%20(1%E2%89%A4t%E2%89%A42%E2%8B%85104)%20%E2%80%94%20the%20number%20of%20test%20cases.%20Then%20t%20test%20cases%20follow.%0A%0AThe%20first%20line%20of%20the%20test%20case%20contains%20one%20integer%20n%20(1%E2%89%A4n%E2%89%A42%E2%8B%85105)%20%E2%80%94%20the%20length%20of%20s.%20The%20second%20line%20of%20the%20test%20case%20contains%20n%20characters%20'0'%20and%20'1'%20%E2%80%94%20the%20string%20s.%0A%0AIt%20is%20guaranteed%20that%20the%20sum%20of%20n%20does%20not%20exceed%202%E2%8B%85105%20(%E2%88%91n%E2%89%A42%E2%8B%85105).%0A%0AOutput%0AFor%20each%20test%20case%2C%20print%20the%20answer%3A%20in%20the%20first%20line%20print%20one%20integer%20k%20(1%E2%89%A4k%E2%89%A4n)%20%E2%80%94%20the%20minimum%20number%20of%20subsequences%20you%20can%20divide%20the%20string%20s%20to.%20In%20the%20second%20line%20print%20n%20integers%20a1%2Ca2%2C%E2%80%A6%2Can%20(1%E2%89%A4ai%E2%89%A4k)%2C%20where%20ai%20is%20the%20number%20of%20subsequence%20the%20i-th%20character%20of%20s%20belongs%20to.%0A%0AIf%20there%20are%20several%20answers%2C%20you%20can%20print%20any.%0A%0AExample%0AinputCopy%0A4%0A4%0A0011%0A6%0A111111%0A5%0A10101%0A8%0A01010000%0AoutputCopy%0A2%0A1%202%202%201%20%0A6%0A1%202%203%204%205%206%20%0A1%0A1%201%201%201%201%20%0A4%0A1%201%201%201%201%202%203%204)

## 思路一

我们发现对于任意si，加入已有子序列bj的唯一要求就是si!=bj[end]。所以我们只需存储目前所有的bj[end]记为bj，对于任意si，遍历各个子序列结尾bj。若si!=bj，则加入；若所有子序列bj==si，则新建一个序列放入。最后结果为序列数

## 代码一（TLE）

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
char s[200010];
int a[200010];
int b[200010];
int main(){
	int t;
	scanf("%d",&t);
	while(t--){
		int ans=0;
		memset(a,0,sizeof(a));
		int n;
		scanf("%d",&n);
		scanf("\n%s",s);
		for(int i=0;i<n;++i){
			a[i]=s[i]-'0';
		}
		for(int i=0;i<n;++i){
			bool flag=true;
			for(int j=1;j<=ans;++j){
				if(a[i]!=b[j]){
					a[i]=j;
					flag=false;
					b[j]+=1;
					b[j]%=2;
					break;
				}
			}
			if(flag){
				++ans;
				b[ans]=a[i];
				a[i]=ans;
			}
		}
		printf("%d\n",ans);
		for(int i=0;i<n;++i){
			printf("%d ",a[i]);
		}
		printf("\n");
	}
	return 0;
}
//	freopen("data.in","r",stdin);
//	freopen("test.out","w",stdout);
```

## 思路二
我们发现超时的原因主要在于搜索可放入序列bj过程耗时过多，最坏情况所有bj!=ai,时间复杂度将退化到O(n^2).由于sum(n)<=2*10^5，最坏情况下t=0，n=200000，时间复杂度为O(200000^2)。显然超时。

针对搜索可放入序列bj过程耗时过多，我们发现用两个数组一个存结尾为0，另一个存结尾为1。每次只需判断数组是否空即可。若不空，则加入序列，并将该序列移至另一数组，否则新建一个序列。进一步思考发现若将数组换位队列将更好操作，于是有了下面这份代码，简单观察可知时间复杂度为O(sum(n))，效率很高。

## 代码二

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
char s[200010];
int a[200010];
queue<int>b0;
queue<int>b1;
int main(){
	int t;
	scanf("%d",&t);
	while(t--){
		while(!b0.empty()){
			b0.pop();
		}
		while(!b1.empty()){
			b1.pop();
		}
		int ans=0;
		memset(a,0,sizeof(a));
		int n;
		scanf("%d",&n);
		scanf("\n%s",s);
		for(int i=0;i<n;++i){
			a[i]=s[i]-'0';
		}
		for(int i=0;i<n;++i){
			if(a[i]==0){
				if(!b0.empty()){
					a[i]=b0.front();
					b0.pop();
					b1.push(a[i]);
				}
				else{
					++ans;
					a[i]=ans;
					b1.push(ans);
				}
			}
			else{
				if(!b1.empty()){
					a[i]=b1.front();
					b1.pop();
					b0.push(a[i]);
				}
				else{
					++ans;
					a[i]=ans;
					b0.push(ans);
				}
			}
		}
		printf("%d\n",ans);
		for(int i=0;i<n;++i){
			printf("%d ",a[i]);
		}
		printf("\n");
	}
	return 0;
}
//	freopen("data.in","r",stdin);
//	freopen("test.out","w",stdout);
```