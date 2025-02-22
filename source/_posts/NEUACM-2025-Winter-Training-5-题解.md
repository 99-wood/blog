---
title: NEUACM 2025 Winter Training 5 题解
date: 2025-02-22 18:35:05
tags:
---

# NEUACM 2025 Winter Training 5 题解

## D 丝之割

### 题意

形式化题意：有 $m$ 条弦，一条弦可以抽象为一个二元组 $(u,v)$，你可以进行任意次切割操作，一次切割操作你将选择两个下标 $i$ 和 $j$ 满足 $i,j \in [1,n]$，然后所有满足 $u>i,v<j$ 的弦 $(u,v)$ 都将被破坏，同时你将付出 $a_i \times b_j​$ 的代价。求破坏所有弦的最小代价和。

### 分析

首先观察到若存在两个弦 $(x, y), (u, v)$ 满足 $x \le u$ 且 $y \ge v$。那么我们在删去 $(x, y)$ 的时候就一定会删去 $(u, v)$。而我们的目标是删除所有的弦，那么就可以忽略 $(u, v)$。

我们假设现在有了 $m$ 个不相交的弦，那就可以定义状态 $f_i$ 表示删除所有小于等于 $i$ 的弦的代价。

$$f_i = \min_{j<i} \left(f_j + \left(\min_{k \in [u_j, u_{j + 1})}a_k \right) \times \left(\min_{k > b_i}a_k \right) \right)$$

我们发现 $\displaystyle \min_{k \in [u_j, u_{j + 1})} a_k$ 只与 $j$ 有关，$\displaystyle \min_{k > b_i}a_k$ 只与 $i$ 有关。不妨用 $p_j$ 表示前者，$q_i$ 表示后者。

$$f_i = \min_{j<i} \left(f_j + p_j \times q_i \right)$$

变形后 $f_j = -q_i \times p_j + f_i$，把 $(p_j, f_j)$ 看作点，$-q_i$ 看作斜率，就是经典的斜率优化求最小截距。

而且从左向右的过程中 $q_i$ 和 $f_j$ 单调不下降，直接用最基本的单调队列求下凸壳即可。

## F 简单题 加强版

### 题意

# P6222 「P6156 简单题」加强版

求

$$\sum_{i=1}^{n}\sum_{j=1}^{n} (i+j)^K \gcd(i,j) \mu^2(\gcd(i,j)) \pmod {2^{32}}$$

要求是 $\mathcal{O}(n)$ 的复杂度。

### 分析

先来点喜闻乐见的莫反套路。

$$
\begin{align*}
    &\sum_{i=1}^{n}\sum_{j=1}^{n} (i+j)^K \gcd(i,j) \mu^2(\gcd(i,j))\\
    =&\sum_{d = 1}^{n}\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor} (id+jd)^K d \mu^2(d)[gcd(i, j)=1]\\
    =&\sum_{d = 1}^{n}d^{K + 1} \mu^2(d)\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor} (i+j)^K \sum_{t|gcd(i, j)}\mu(t)\\
    =&\sum_{d = 1}^{n}d^{K + 1} \mu^2(d)\sum_{t=1}^{\lfloor\frac{n}{d}\rfloor}\mu(t)\sum_{i=1}^{\lfloor\frac{n}{dt}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{dt}\rfloor} (it+jt)^K\\
    =&\sum_{d = 1}^{n}d^{K + 1} \mu^2(d)\sum_{t=1}^{\lfloor\frac{n}{d}\rfloor}\mu(t)t^{K}\sum_{i=1}^{\lfloor\frac{n}{dt}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{dt}\rfloor}  (i+j)^K\\
\end{align*}
$$

以上都是莫反的常规操作，下面进入本题的核心。

令 $T = dt$，有

$$
\begin{align*}
    &\sum_{d = 1}^{n}d^{K + 1} \mu^2(d)\sum_{t=1}^{\lfloor\frac{n}{d}\rfloor}\mu(t)t^{K}\sum_{i=1}^{\lfloor\frac{n}{dt}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{dt}\rfloor}  (i+j)^K\\
    =&\sum_{T = 1}^{n}T^{K} \sum_{d|T}d\mu^2(d)\mu(\frac{T}{d})\sum_{i=1}^{\lfloor\frac{n}{T}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{T}\rfloor}  (i+j)^K
\end{align*}
$$
。

这里还需要用一个技巧就是自然数幂次可以用线筛在 $\mathcal{O}(n)$ 时间处理。其实做法也比较显然，因为这是一个完全积性函数，所以只需要用快速幂算质数的幂次，剩下的用完全积性函数的性质算。

$\displaystyle \sum_{i=1}^{\lfloor\frac{n}{T}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{T}\rfloor}  (i+j)^K$ 这部分显然可以预处理出来。

我们用 $\mathrm{id}^{K}_i$ 表示我们刚才算出来的自然数幂。求前缀和得到 $f_i$。用 $F_i$ 表示 $\displaystyle \sum_{j=1}^{i}\sum_{k=1}^{i} (j + k)^K$。

我们稍微在纸上写一写，容斥一下有 
$$F_i = F_{i-1} + 2(f_{2i-1} - f_{i}) + \mathrm{id}^K_{2i}$$
。

现在剩下 $\displaystyle \sum_{d|T}d\mu^2(d)\mu(\frac{T}{d})$。

我们发现这实际上就是一个积性函数的迪利克雷卷积，而积性函数的迪利克雷卷积也是积性函数。根据积性函数的性质，我们只需要求出质数的幂次处的值就能用欧拉筛在线性时间算完整个函数。我们令 $\displaystyle g_i = \sum_{d|i}d\mu^2(d)\mu(\frac{i}{d})$。下面我们讨论这个函数在质数的幂次处的值。

- 当 $k=0$ 时，$g(p^k) = g(1) = 1$。
- 当 $k=1$ 时，$g(p^k) = g(p) = \mu^2(1)\mu(p) + p\mu^2(p)\mu(1) = -1 + p$。
- 当 $k=2$ 时，$g(p^k) = g(p^2) = \mu^2(1)\mu(p^2) + p\mu^2(p)\mu(p) + p^2\mu^2(p^2)\mu(1) = -p$。
- 当 $k>2$ 时，无论怎么选 $d$，$\mu^2(d)$ 和 $\mu(\frac{i}{d})$ 中至少有一个有高次幂，所以 $g(p^k)=0$。

现在，原式就变成了

$$
\begin{align*}
    \sum_{T = 1}^{n}\mathrm{id}^K(T) g(T)F\left(\lfloor\frac{n}{T}\rfloor \right)
\end{align*}
$$

就可以用经典的数论分块求解了！

## H String Problem

### 题意

给定一个字符串，求出每个前缀当中字典序最大的子串。

### 分析

稍加分析我们就可以得出一个比较显然的结论，一个字符串当中字典序最大的子串一定是它的一个后缀。现在我们只需要求每个前缀当中字典序最大的后缀。

如果要进一步解决问题就需要进一步观察，这里我列一个我认为对我启发比较大的观察。

如果我们已经求出当前前缀中字典序最大的后缀，现在尾部再加入一个字符，新的答案要么不变，要么就是新加入的字母，要么是 **原答案的最小 border 加上这个字符**。这里 border 指的是当前答案的与其相同前缀的后缀。

我们知道 border 与循环节有着密不可分的关系。如果我们知道一个字符串的循环节，就能直接找到它的最小 border。

我们假设已经求出了长度为 $i$ 的前缀的答案为 $p$ 开始的后缀，且它的循环节长度为 $k$，考虑新加入字符 $s_{i+1}$ 对答案的影响。

若 $s_{i+1} > s_p$，那么新答案就是 $i + 1$，因为其他字符一定都小于 $s_{i+1}$。

若 $s_{i+1} > s_{p + ((i + 1 - p) \bmod k)}$，也就是新字符比循环节中对应的字符大，那么新答案就是 $i + 1 - ((i + 1 - p) \bmod k)$。稍微写一写就能看出，就不证明了。

若 $s_{i+1} < s_{p + ((i + 1 - p) \bmod k)}$，也就是新字符比循环节中对应的字符小，那么答案不变，但是要重新求循环节。

若 $s_{i+1} < s_{p + ((i + 1 - p) \bmod k)}$，也就是新字符就是循环节中对应的字符，那么答案不变，循环节也不变。

至于怎么求循环节，我采用了 $\mathrm{Z}$ 函数的方法。循环节的长度就是第一个相同前缀是自己本身的后缀与整个字符串的差。我们找到第一个就不用再求了，因为我们只需要求循环节。然后整个更新过程中求 $\mathrm{Z}$ 函数都是不断往右的，复杂度也是线性的。


## L Pastoral Oddities

### 题意

给定一张 $n$ 个点的无向图，初始没有边。

依次加入 $m$ 条带权的边，每次加入后询问是否存在一个边集，满足每个点的度数均为奇数。

若存在，则还需要最小化边集中的最大边权。

### 分析

本题最重要的观察就是原命题等价于存在一个边集使得只存在大小为偶数的连通块。

必要性：若存在大小为奇数的连通块，每个点度数又是奇数，那边数就是 $\displaystyle \frac{\sum \mathrm{deg}_i}{2}$，无法除尽，矛盾。

充分性：对于任意的大小为偶数的连通块，取一个它的生成树，对于每个非根节点，若它儿子给它的度数为偶，则连父亲，否则不连父亲。这样可以保证除根以外都满足度数为奇数。再用刚才的方法，边数等于 $\displaystyle \frac{\sum_{i \ne \mathrm{root}} \mathrm{deg}_i + \mathrm{deg}_\mathrm{root}}{2}$。分子中前一项为奇数，后一项一定也是奇数。

在有了这个结论后，原问题就转化为要最小化边集中的最大边权使得子图中连通块大小都为偶数。如果是静态的问题，可以按边权排序，用并查集维护每个连通块大小，不断加边知道满足条件。

动态的情况下，一般与连通块并查集相关的问题都可以尝试用线段树分治解决。

我们给边排序，考虑在全部边都可用的情况下算出答案就是最后一次加入边的答案。我们考虑倒序删边，已加入的边在被删掉前一定都在答案中。

我们考虑线段树分治，先递归右子树，再递归左子树，到叶子节点时，不断加边知道满足条件，期间加入的边在由于时间而删掉前会一直存在，所以可以边加入，边修改线段树，将这条边一定在答案中的区间中都加上这条边。