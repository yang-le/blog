---
date:
  created: 2026-08-23
categories:
    - 数学
    - 算法
---

# 莫比乌斯反演、容斥原理和离散微积分基本定理

## 偏序集

### 偏序关系

称一个关系为偏序关系，记作$\preccurlyeq$，如果它满足

<!-- more -->

- 自反性：$a \preccurlyeq a$
- 反对称：若$a \preccurlyeq b$且$b \preccurlyeq a$，则$a = b$
- 传递性：若$a \preccurlyeq b$且$b \preccurlyeq c$，则$a \preccurlyeq c$

??? note "什么叫做（二元）关系？"

    一个从$A$到$B$的二元关系$R$，是笛卡尔积$A \times B$的子集

    如果$(a, b) \in \preccurlyeq$，我们就说$a$与$b$具有关系$\preccurlyeq$，通常记作$a \preccurlyeq b$.

### 偏序集上的莫比乌斯函数

考虑一个偏序集$P$，即带有偏序关系$\preccurlyeq$的集合，递归地定义其莫比乌斯函数$\mu$:

1. $\mu(s, s) = 1, ~ \forall s \in P$
2. $\displaystyle \mu(s, u) = -\sum_{s \preccurlyeq t \prec u}\mu(s, t), ~ \forall s, u \in P, ~ s \prec u$

其中$s \prec u$的含义是$s \preccurlyeq u$且$s \neq u$.

### 莫比乌斯反演

接着上面的定义，设$f, g : P \to K$，其中$K$是一个交换环，则

$$g(t) = \sum_{s \preccurlyeq t}f(s), ~ \forall t \in P$$

当且仅当

$$f(t) = \sum_{s \preccurlyeq t}g(s)\mu(s, t), ~ \forall t \in P$$

## 例子

### 自然数集$\N$和其上的偏序关系$\le$

在这个偏序集设定下，有

$$\mu(s, u) = - \sum_{s \le t < u}\mu(s, t), ~ \forall s < u$$

所以$\mu(s, s + k) = -[\mu(s, s) + \mu(s, s + 1) + \cdots \mu(s, s + k - 1)]$，其中$k \ge 1$.

当$k = 1$时，$\mu(s, s + 1) = -\mu(s, s) = -1$.

而当$k \ge 2$时有递推关系$\mu(s, s + k) = \mu(s, s + k - 1) - \mu(s, s + k - 1) = 0$.

故这里的$\mu(s, u)$可以显示定义为

$$\mu(s, u) = \begin{cases}
1, &u = s \\
-1, & u = s + 1 \\
0, & u \ge s + 2
\end{cases}, ~ \forall s \le u$$

莫比乌斯反演关系表现为

$$g(t) = \sum_{s \le t}f(s)$$

当且仅当

$$f(t) = \sum_{s \le t}g(s)\mu(s, t) = g(t) - g(t - 1)$$

这就是离散微积分基本定理，或者说前缀和与差分的关系。

### 幂集$\mathcal P(S)$及其上的偏序关系$\subseteq$

在这个偏序集设定下，有

$$\mu(A, B) = - \sum_{A \subseteq C \subset B}\mu(A, C), ~ \forall A \subset B$$

可以用归纳法证明$\mu(A, B) = (-1)^{|B| - |A|}, ~ \forall A \subseteq B$

??? info "证明"

    设$d = |B| - |A|$，基础情况是$d = 0$，结合$A \subseteq B$得$A = B$，此时有$\mu(A, A) = 1 = (-1)^d$

    现在假设对于所有满足$|C| - |A| < d$的$C$（其中$A \subseteq C \subset B$）都有$\mu(A, C) = (-1)^{|C| - |A|}$

    设$|A| = a, |B| = b$，差值$d = b - a$，那么任意中间集合$C$的大小$c$满足$a \le c < b$

    考虑对于每个固定的$c$，有多少个$C$满足$|C| = c$且$A \subseteq C \subset B$. 显然$A$中的$a$个元素已经都在$C$中了，还需要从$B - A$中选择$c - a$个元素加入到$C$中，因此满足条件的$C$的数量是$d \choose {c - a}$，于是

    $$\sum_{A \subseteq C \subset B}\mu(A, C) = \sum_{c = a}^{b - 1}{d\choose c - a}(-1)^{c - a} = \sum_{k = 0}^{d - 1}{d\choose k}(-1)^k$$

    根据二项式定理

    $$\sum_{k = 0}^d{d\choose k}(-1)^k = (1 - 1)^d = 0$$

    得到

    $$\sum_{k = 0}^{d - 1}{d\choose k}(-1)^k = \sum_{k = 0}^d{d\choose k}(-1)^k - {d\choose d}(-1)^d = -(-1)^d$$

    因此

    $$\mu(A, B) = - \sum_{A \subseteq C \subset B}\mu(A, C) = (-1)^d = (-1)^{|B| - |A|}$$

考虑一个新的偏序关系$\supseteq$，它定义为$A \supseteq B$当且仅当$B \subseteq A$。莫比乌斯反演关系在这个偏序下表现为

$$g(B) = \sum_{A \supseteq B}f(A)$$

当且仅当

$$f(B) = \sum_{A \supseteq B}g(A)\mu'(A, B)$$

其中$\mu'(A, B)$和$\mu(A, B)$的定义是一样的，只是将定义中的关系$\subseteq$改为$\supseteq$，于是$\mu'(A, B) = \mu(B, A)$

把$S$看作性质的集合，这里$g, f$都是计数函数，$f(A)$是**恰好**满足性质集合$A$的元素数量，$g(B)$是**至少**满足性质集合$B$的元素数量。

``` mermaid
venn-beta
    set A[英]
    set B[法]
    set C[德]
    union A,B[英法]
    union B,C[法德]
    union A,C[英德]
    union A,B,C[英法德]
    style A,B fill:skyblue
    style B,C fill:orange
    style A,C fill:lightgreen
    style A,B,C fill:white
```

比如：$g(E) = f(E) + f(E \cap G) + f(E \cap F) + f(E \cap F \cap G)$

???+ note "注"

    至少会英语的人（蓝色边框） = 恰好只会英语的人（蓝色） + 恰好只会英语和德语的人（绿色） + 恰好只会英语和法语的人（青色） + 恰好只会英语法语德语的人（白色）

于是$f, g$的关系满足$g(B) = \sum_{A \supseteq B}f(A)$，根据莫比乌斯反演就有

$$f(B) = \sum_{A \supseteq B}\mu(B, A)g(A)$$

比如：$f(E) = g(E) - g(E \cap G) - g(E \cap F) + g(E \cap F \cap G)$

???+ note "注"

    恰好只会英语的人（蓝色） = 至少会英语的人（蓝色边框） - 至少会英语和德语的人（绿+白） - 至少会英语和法语的人（青+白） + 至少会英语法语德语的人（白色）

特别地，若$B = \empty$，我们有

$$g(\empty) = \sum_Af(A)$$

当且仅当

$$f(\empty) = \sum_A(-1)^{|A|}g(A)$$

这里$g(\empty)$表示全集$U$，而$f(\empty)$表示一种语言都不会的人。变一下形，得到

$$g(\empty) - f(\empty) = \sum_{A \neq \empty}(-1)^{|A| + 1}g(A)$$

即至少会一种语言的人数的计算方法：

$$|E \cup F \cup G| = |E| + |F| + |G| - |E \cap G| - |E \cap F| - |F \cap G| + |E \cap F \cap G|$$

这正是容斥原理，幂集上的莫比乌斯反演的一个特殊情况。
