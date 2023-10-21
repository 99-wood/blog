---
title: 板刷 Atcoder Educational DP Contest 总结
date: 2023-10-21 23:03:56
categories:
- [DP]
tags:
- [AtCoder]
- [题解]
---

# 板刷 Atcoder Educational DP Contest 总结

## 前言

最近为了水 ACM 选修课积分, 于是写了一下 OJ 中的 DP 提高题, 发现是 AtCoder 上的题目. 做下来收获颇丰, 所以写篇总结.

总结会写一些我在写的时候花时间比较多或者看了题解或者有收获的题, 代码选择性地放.

## 正文


### J Sushi

[题目链接](https://www.luogu.com.cn/problem/AT_dp_j)

难在优化状态, 发现答案与盘子的顺序无关, 设计状态 $f_{i, j, k}$ 表示有 $i$ 个盘子有 $1$ 个寿司, 有 $j$ 个盘子有 $2$ 个寿司, 有 $k$ 个盘子有 $3$ 个寿司全部吃光的期望次数. 

状态转移方程推出来是

$$f_{i, j, k} = \frac{i}{i + j + k} \times f_{i - 1, j, k}  + \frac{j}{i + j + k} \times f_{i + 1, j - 1, k}  + \frac{k}{i + j + k} \times f_{i, j + 1, k - 1}  + \frac{n}{i + j + k}$$

一个易错点是循环顺序是 $k$, $j$, $i$. 读者可以思考一下原因.

### R Walk

[题目链接](https://www.luogu.com.cn/problem/AT_dp_r)

设计状态 $f_{i, u, v}$ 表示长度为 $i$ 的从 $u$ 到 $v$ 的路径个数.

转移方程

$$f_{i, u, v} = \sum\limits_{k = 1}^n f_{i - 1, u, k} + f_{1, k, v}$$

这东西很像矩阵乘法, 可以用矩阵快速幂优化.

### U Grouping

[题目链接](https://www.luogu.com.cn/problem/AT_dp_u)

枚举子集的方法

```cpp
for(int i = now; i; i = (i - 1) & now){
	f[now] = max(f[now], f[now ^ i] + solve(i));
}
```

### V Subtree

[题目链接](https://www.luogu.com.cn/problem/AT_dp_v)

换根 DP. 但是模数不一定为质数, 所以记录前缀积和后缀积解决.

### W Intervals

[题目链接](https://www.luogu.com.cn/problem/AT_dp_w)

好题.

按区间右端点排序, 只有考虑到 i 时, 计算所有右端点为 i 的区间对答案的贡献, 可以做到不重不漏. 而且可以发现, 这样计算只需要考虑最右边选的点的位置.

令 $f_{i,j}$ 表示考虑到 i, 最后一个点在 j 的最大值.

写出状态转移方程发现可以把 $f$ 放在线段树上. 很好的一道题.

### Y Grid 2

[题目链接](https://www.luogu.com.cn/problem/AT_dp_y)

带障碍的走方格. 一开始想离散化做, 发现代码困难遂看题解.

设计状态 $f_{i}$ 表示从开始到障碍 $i$ 的位置**且不通过障碍**的方案数.

$$f_{i} = \tbinom{x_i - 1 + y_i - 1}{x_i - 1} - \sum\limits_{x_j \leq x_i, y_j \leq y_i} \tbinom{x_i - x_j + y_i - y_j}{x_i - x_j} f_{j} $$

### Z Frog 3

[题目链接](https://www.luogu.com.cn/problem/AT_dp_z)

斜率优化 DP 模板题.