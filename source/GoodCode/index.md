---
title: Good Code
date: 2020-03-01 23:15:29
---
#Good Code

##技巧

###快速求log2(x)
```C++
int mylog(int n) {
	int result = 0;  
	if(n&0xffff0000) {result += 16; n >>= 16; }  
	if(n&0x0000ff00) {result += 8; n >>= 8; }  
	if(n&0x000000f0) {result += 4; n >>= 4; }  
	if(n&0x0000000c) {result += 2; n >>= 2; }  
	if(n&0x00000002) {result += 1; n >>= 1; }  
	return result; 
}
```
