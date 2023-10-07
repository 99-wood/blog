---
title: 2021 ICPC Asia Taiwan Online Programming Contest 题解
date: 2023-10-05 21:15:42
categories:
- [DP]
- [数学]
- [数据结构, 线段树]
- [图论, 网络流]
tags:
- [Codeforces]
- [题解]
---
# 2021 ICPC Asia Taiwan Online Programming Contest 题解

训练题, 场上做了 ABDFG. 目前补了 CJ. 比较可惜是 J 题是签到题, 放在最后没来得及看, 经验问题.

题目链接: [https://codeforces.com/group/nzwf9tsg2d/contest/477278](https://codeforces.com/group/nzwf9tsg2d/contest/477278)

## A Olympic Ranking

### 分析

一眼顶针

### 代码

[GitHub](https://github.com/99-wood/OIcode/blob/main/CF/2021%20ICPC%20Asia%20Taiwan%20Online%20Programming%20Contest%20%26%20NEU%20training%202023%20Autumn%20Contest%2014%20(Individual%20only)/a.cpp)

## B Aliquot Sum

### 分析

求因数和, 可以对每个数枚举因子, 复杂度 $O(n\sqrt{n})$.

如果用筛法, 对于每个数筛它的倍数, 可以做到 $O(n\log{n})$. 证明需要用到调和级数的增长近似于 $\ln(n) + C$, 其中 $C$ 为 欧拉常数.

### 代码

[GitHub](https://github.com/99-wood/OIcode/blob/main/CF/2021%20ICPC%20Asia%20Taiwan%20Online%20Programming%20Contest%20%26%20NEU%20training%202023%20Autumn%20Contest%2014%20(Individual%20only)/b.cpp)

## C A Sorting Problem

### 分析

考场上往这个方向想, 但没深入, 就先做后面了. 还是生疏了.

具体做法是记录每个数的位置, 问题转化为每次操作交换相邻两数, 使得数组单调递增. 经典求逆序对数量. 使用归并排序或者树状数组即可.

### 代码

[GitHub](https://github.com/99-wood/OIcode/blob/main/CF/2021%20ICPC%20Asia%20Taiwan%20Online%20Programming%20Contest%20%26%20NEU%20training%202023%20Autumn%20Contest%2014%20(Individual%20only)/c.cpp)

## D Drunk Passenger

### 分析

这道题花了不少时间, 还是太菜了.

考虑 DP. 假设当前已经坐了 $i$ 个人, 一个显然的结论是 $2$ 到 $i$ 的座位已经坐了人, 而 $i + 1$ 到 $n$ 的座位在当前状态是等价的, 还有第一个座位比较特殊. 所以定义 $f_i$ 表示坐了 $i$ 个人后本就不应坐人的座位 (也就是 $i + 1$ 到 $n$ 的座位) 坐人的概率, $g_i$ 表示坐了 $i - 1$ 个人后第一个座位坐人的概率. 有如下转移:

$$f_i = f_{i - 1} + f_{i - 1} \times \frac{1}{n - (i - 1)}$$

$$g_i = g_{i - 1} + f_{i - 1} \times \frac{1}{n - (i - 1)}$$

当然, 你也可以推式子. 用题解的话说就是

>推出答案之數學式子 $\frac{n}{2n - 1}$ 直接算答案。

~台湾的题解挺有意思.~

### 代码

[GitHub](https://github.com/99-wood/OIcode/blob/main/CF/2021%20ICPC%20Asia%20Taiwan%20Online%20Programming%20Contest%20%26%20NEU%20training%202023%20Autumn%20Contest%2014%20(Individual%20only)/d.cpp)

## E Eatcoin

### 分析

python 题.

首先求自然数五次方和公式, 可以用拉格朗日手推, 也可以写前几项高斯消元.

然后二分法求最小的天数.

### 代码

没有, 谁愿意拿 python 写写吧, 二分模板题.

## F Flip

### 分析

和动态区间和最值很像.

维护区间左右端点的值, 从左和从右开始满足条件最长长度, 区间内答案数就可以用线段树维护了.

### 代码

[GitHub](https://github.com/99-wood/OIcode/blob/main/CF/2021%20ICPC%20Asia%20Taiwan%20Online%20Programming%20Contest%20%26%20NEU%20training%202023%20Autumn%20Contest%2014%20(Individual%20only)/f.cpp)

## G Garden Park

### 分析

正解 DP. 找官方题解吧.

我拿点分治做的, 模板题.

统计经过当前树根的答案数, 点分治换树根即可.

核心在于求经过当前树根的答案数. 对每个儿子 dfs, 找出所有单调递减和单调递增的路径数, 然后分别用乘法原理算贡献. 复杂度 $O(n\log^2{n})$.

### 代码

[GitHub](https://github.com/99-wood/OIcode/blob/main/CF/2021%20ICPC%20Asia%20Taiwan%20Online%20Programming%20Contest%20%26%20NEU%20training%202023%20Autumn%20Contest%2014%20(Individual%20only)/g.cpp)

## H A Hard Problem

### 分析

显然要分开每一位考虑. 然后要求满足条件的代价最小的染色方法.

跟所有的网络流题目一样, 然后它就是最小割了.

虽然对于会网络流的这是模板题, 但上次写网络流是一年前, 所以等于不会.

### 代码

以后补.

补好了.

[GitHub](https://github.com/99-wood/OIcode/blob/main/CF/2021%20ICPC%20Asia%20Taiwan%20Online%20Programming%20Contest%20%26%20NEU%20training%202023%20Autumn%20Contest%2014%20(Individual%20only)/h.cpp)

## G ICPC Kingdom

以后补

## J JavaScript

### 分析

签到题, 判断是不是全是数字.

### 代码

[GitHub](https://github.com/99-wood/OIcode/blob/main/CF/2021%20ICPC%20Asia%20Taiwan%20Online%20Programming%20Contest%20%26%20NEU%20training%202023%20Autumn%20Contest%2014%20(Individual%20only)/j.cpp)