---
title: NEUACM 2025 Winter Training 6 题解
date: 2025-02-25 15:26:53
tags:
---


# NEUACM 2025 Winter Training 6 题解

## D 和平委员会 & E Riddle

第一次接触 2-SAT 问题，感觉网上的很多博客讲的都不是很清楚，搞了半天没搞懂，好在最后搞懂了。

SAT 问题是指有若干个变元，和一个谓词，要求找出一个指派使得谓词为真，或者说明无解。

2-SAT 问题指的是把这个谓词写成合取范式后，每个大项中最多有两个变元情形。只有 2-SAT 可以在多项式时间复杂度内求解。

求解的具体方法就是把每个的变元的真与假看作点，大项改写为蕴含式，然后将前见代表的点连一条指向后见的边。若某个变元的真与假在同一强连通分量中，那么无解。反之，如果不存在这样情况，对于我们取缩点后编号较小的点作为该变元的指派，就可以构造出一种解。

这样做的道理在于，当这样构造图后，在图上的移动相当于假定前见为真来进行推理。如果某个变元的真与假在同一强连通分量中，说明无论我们对这个变元进行何种指派，都会推出矛盾。而取缩点后编号较小的点是因为编号较小的点在缩点过程中是后访问的，对编号较大的点没有影响。

### 题意

根据宪法，Byteland 民主共和国的公众和平委员会应该在国会中通过立法程序来创立。 不幸的是，由于某些党派代表之间的不和睦而使得这件事存在障碍。 此委员会必须满足下列条件：

- 每个党派都在委员会中恰有 $1$ 个代表。
- 如果 $2$ 个代表彼此厌恶，则他们不能都属于委员会。

每个党在议会中有 $2$ 个代表。代表从 $1$ 编号到 $2n$。 编号为 $2i-1$ 和   $2i$ 的代表属于第 $i$ 个党派。 

任务：写一程序读入党派的数量和关系不友好的代表对，计算决定建立和平委员会是否可能，若行，则列出委员会的成员表。

### 分析

在第 $i$ 个党派中钦定一个成员是否去看作变元 $X_i$，那么另一个成员是否去就是 $\neg X_i$。

若 $i$ 和 $j$ 中我们钦定的人厌恶，那么谓词就是 $\neg X_i \vee \neg X_j$，写成蕴含式就是 $X_i \rightarrow \neg X_j$ 和 $X_j \rightarrow \neg X_i$。

感觉理解了 2-SAT 后还是比较好写的。

## F Colorful Tree

### 题意
有一个 $N$ 个节点的树，每条边有颜色、边权。 您需要处理 $Q$ 个询问，每个询问给出 $x_i, y_i, u_i, v_i$，您需要求出假定所有颜色为 $x_i$​ 的边边权全部变成 $y_i$ 后，$u_i$ 和 $v_i$ 之间的距离。询问之间互相独立。

### 分析

问题转化后就是要求询问的链上指定颜色的个数和边权和。当然可以用主席树解决。

但是莫队也是一种方法。受到之前一道题目启发，一个数上的链可以看作欧拉序中的一段括号序列，那我们可以把要求的链对应到欧拉序上，初始括号序列为空，不断扩展。若没有匹配，则加上这条边的贡献，若有匹配，表明要现有括号序列要弹出，也就是减掉这条边的贡献。

括号序列扩展容易，不好删除，所以可以用不回滚莫队解决。

## G Rabbit Exercise

### 题意

有 $n$ 只兔子在一个数轴上，兔子为了方便起见从 $1$ 到 $n$ 标号，第 $i$ 只兔子的初始坐标为 $x_i$。兔子会以以下的方式在数轴上锻炼：一轮包含 $m$ 个跳跃，第 $j$ 次跳跃是兔子 $a_j (2 \le a_j \le N−1)$ 跳一下，这一下从兔子 $a_{j−1}$ 和兔子 $a_{j+1}$ 中等概率的选一个。假设选了 $x$，那么 $a_j$ 号兔子会跳到它当前坐标关于 $x$ 的坐标的对称点。 注意，即使兔子的位置顺序变化了，但是编号仍保持不变，这里按兔子编号算。兔子会进行 $k$ 轮跳跃，对每个兔子，请你求出它最后坐标的期望值。

### 分析

兔子跳跃后的期望 $\displaystyle E(x_i') = \frac{1}{2}(2x_{i - 1} - x_i) + \frac{1}{2}(2x_{i - 1} - x_i) = x_{i-1} + x_{i+1} - x_i$。根据期望的线性性可以计算。但是轮数很多，$n$ 也很大，甚至无法用矩阵快速幂解决。

但是有个惊人的注意到：$x_i' - x_{i-1} = x_{i+1} - x_i$ 以及 $x_i - x_{i-1} = x_{i+1} - x_i'$，也就是每次跳跃只会交换差分。那我们可以把交换写成置换的形式，然后用快速幂就好了！

## H Sky Full of Stars

### 题意

有一个 $n\times n(n \le 10^6)$ 的正方形网格，用红色，绿色，蓝色三种颜色染色，求有多少种染色方案使得至少一行或一列是同一种颜色。结果对 $998244353$ 取模。

### 分析

设 $A_i$ 为所有着色的集合，其中第 $i$ 行仅包含一种颜色，$B_i$ 为所有着色的集合，其中第 $i$ 列仅包含一种颜色。

因此，需要计算 $|A_1 \cup A_2 \cup \dots \cup A_n \cup B_1 \cup B_2 \cup \dots \cup B_n|$。

这个就可以用容斥原理计算了。

$$
\text{ans} = \sum_{i=0, j = 0, i+j>0}^{n} \binom{n}{i} \binom{n}{j} (-1)^{i+j+1} f(i,j)
$$

其中 $f(i,j)$ 是满指定 $i$ 行和前 $j$ 列是同种颜色的的着色数。

$f(0,k) = 3^k \cdot 3^{n(n-k)}$。
如果 $i, j$ 都大于零，那么我们可以注意到，所有选定的行和列实际上是同一种颜色。

因此，$f(i,j) = 3 \cdot 3^{(n-i)(n-j)}$。我们先 $\mathcal{O}(n)$ 计算 $i, j$ 有一个是 $0$ 的部分，剩下的就是：

$$
\begin{align*}
    \text{ans} =& \sum_{i=1}^{n} \sum_{j=1}^{n} \binom{n}{i} \binom{n}{j} (-1)^{i+j+1} 3 \cdot 3^{(n-i)(n-j)} \\
    =&3\sum_{i=1}^{n} \binom{n}{i}(-1)^{i+1}  \sum_{j=1}^{n} \binom{n}{j} (-1)^{j} {(3^{(n-i)})}^{(n-j)}\\
    =&3\sum_{i=1}^{n} \binom{n}{i}(-1)^{i+1} \left(\left(3^{n-i} - 1 \right)^n - 3^{(n-i)n}\right)
\end{align*}
$$

又可以 $\mathcal{O}(n)$ 计算了！
