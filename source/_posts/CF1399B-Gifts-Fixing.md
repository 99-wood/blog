---
title: CF1399B Gifts Fixing
date: 2020-08-06 18:16:31
categories: "入门"
tags: "入门"
---

# CF1399B Gifts Fixing

##题目

	B. Gifts Fixing
	time limit per test1 second
	memory limit per test256 megabytes
	inputstandard input
	outputstandard output
	You have n gifts and you want to give all of them to children. Of course, you don't want to offend anyone, so all gifts should be equal between each other. The i-th gift consists of ai candies and bi oranges.
	
	During one move, you can choose some gift 1≤i≤n and do one of the following operations:
	
	eat exactly one candy from this gift (decrease ai by one);
	eat exactly one orange from this gift (decrease bi by one);
	eat exactly one candy and exactly one orange from this gift (decrease both ai and bi by one).
	Of course, you can not eat a candy or orange if it's not present in the gift (so neither ai nor bi can become less than zero).
	
	As said above, all gifts should be equal. This means that after some sequence of moves the following two conditions should be satisfied: a1=a2=⋯=an and b1=b2=⋯=bn (and ai equals bi is not necessary).
	
	Your task is to find the minimum number of moves required to equalize all the given gifts.
	
	You have to answer t independent test cases.
	
	Input
	The first line of the input contains one integer t (1≤t≤1000) — the number of test cases. Then t test cases follow.
	
	The first line of the test case contains one integer n (1≤n≤50) — the number of gifts. The second line of the test case contains n integers a1,a2,…,an (1≤ai≤109), where ai is the number of candies in the i-th gift. The third line of the test case contains n integers b1,b2,…,bn (1≤bi≤109), where bi is the number of oranges in the i-th gift.
	
	Output
	For each test case, print one integer: the minimum number of moves required to equalize all the given gifts.
	
	Example
	input
	5
	3
	3 5 6
	3 2 3
	5
	1 2 3 4 5
	5 4 3 2 1
	3
	1 1 1
	2 2 2
	6
	1 1000000000 1000000000 1000000000 1000000000 1000000000
	1 1 1 1 1 1
	3
	10 12 8
	7 5 4
	output
	6
	16
	0
	4999999995
	7
	Note
	In the first test case of the example, we can perform the following sequence of moves:
	
	choose the first gift and eat one orange from it, so a=[3,5,6] and b=[2,2,3];
	choose the second gift and eat one candy from it, so a=[3,4,6] and b=[2,2,3];
	choose the second gift and eat one candy from it, so a=[3,3,6] and b=[2,2,3];
	choose the third gift and eat one candy and one orange from it, so a=[3,3,5] and b=[2,2,2];
	choose the third gift and eat one candy from it, so a=[3,3,4] and b=[2,2,2];
	choose the third gift and eat one candy from it, so a=[3,3,3] and b=[2,2,2].

## 翻译

[百度翻译结果](https://fanyi.baidu.com/?aldtype=16047#en/zh/A.%20Remove%20Smallest%0Atime%20limit%20per%20test1%20second%0Amemory%20limit%20per%20test256%20megabytes%0Ainputstandard%20input%0Aoutputstandard%20output%0AYou%20are%20given%20the%20array%20a%20consisting%20of%20n%20positive%20(greater%20than%20zero)%20integers.%0A%0AIn%20one%20move%2C%20you%20can%20choose%20two%20indices%20i%20and%20j%20(i%E2%89%A0j)%20such%20that%20the%20absolute%20difference%20between%20ai%20and%20aj%20is%20no%20more%20than%20one%20(%7Cai%E2%88%92aj%7C%E2%89%A41)%20and%20remove%20the%20smallest%20of%20these%20two%20elements.%20If%20two%20elements%20are%20equal%2C%20you%20can%20remove%20any%20of%20them%20(but%20exactly%20one).%0A%0AYour%20task%20is%20to%20find%20if%20it%20is%20possible%20to%20obtain%20the%20array%20consisting%20of%20only%20one%20element%20using%20several%20(possibly%2C%20zero)%20such%20moves%20or%20not.%0A%0AYou%20have%20to%20answer%20t%20independent%20test%20cases.%0A%0AInput%0AThe%20first%20line%20of%20the%20input%20contains%20one%20integer%20t%20(1%E2%89%A4t%E2%89%A41000)%20%E2%80%94%20the%20number%20of%20test%20cases.%20Then%20t%20test%20cases%20follow.%0A%0AThe%20first%20line%20of%20the%20test%20case%20contains%20one%20integer%20n%20(1%E2%89%A4n%E2%89%A450)%20%E2%80%94%20the%20length%20of%20a.%20The%20second%20line%20of%20the%20test%20case%20contains%20n%20integers%20a1%2Ca2%2C%E2%80%A6%2Can%20(1%E2%89%A4ai%E2%89%A4100)%2C%20where%20ai%20is%20the%20i-th%20element%20of%20a.%0A%0AOutput%0AFor%20each%20test%20case%2C%20print%20the%20answer%3A%20%22YES%22%20if%20it%20is%20possible%20to%20obtain%20the%20array%20consisting%20of%20only%20one%20element%20using%20several%20(possibly%2C%20zero)%20moves%20described%20in%20the%20problem%20statement%2C%20or%20%22NO%22%20otherwise.)

## 思路

**~~自己想~~**

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
int b[100];
int main(){
	int t;
	scanf("%d",&t);
	while(t--){
		int n;
		scanf("%d",&n);
		int mina=1000000010,minb=1000000010;
		for(int i=0;i<n;++i){
			scanf("%d",&a[i]);
			if(a[i]<mina)mina=a[i];
		}
		for(int i=0;i<n;++i){
			scanf("%d",&b[i]);
			if(b[i]<minb)minb=b[i];
		}
		long long con=0;
		for(int i=0;i<n;++i){
			int x=a[i]-mina,y=b[i]-minb;
			con+=max(x,y);
		}
		printf("%I64d\n",con);
	}
	return 0;
}
//	freopen("data.in","r",stdin);
//	freopen("test.out","w",stdout);
```