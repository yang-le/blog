---
date:
  created: 2026-11-15
categories:
    - 数学
    - 算法
---

# MiniLean4

## Curry-Howard 同构

- 一个命题就是一个类型（反之不一定）
- 命题的一个证明就是类型的一个项(term)
- 命题$A$为真当且仅当类型$A$的某个项$t$存在($A$ is inhabited)，写作$t : A$.

> 证明一个定理意味着构造对应类型的一个项，验证一个证明意味着对项做类型检查。

<!-- more -->

## 项的种类

- $Sort~u$是宇宙类型，其元素也是类型。其中$u$是宇宙等级，$Sort~0$是命题宇宙。
- $x$是变量。
- $\Pi(x : A).B$是依赖函数类型。它是以$x : A$为参数且返回值类型为$B$的函数的类型，其中$B$可能依赖于$x$.
- $\lambda(x : A).t$是$\lambda$抽象。它是以$x : A$为参数且返回$t$的函数。
- $t~s$是函数应用，将函数$t$作用于参数$s$上。

### 约定

1. 箭头：当$B$不依赖$x$时，我们将$\Pi(x : A).B$简写为$A \to B$. 于是普通函数类型只是依赖函数类型的一种特列；蕴含只是一种特殊的全称量词。
2. 结合性：函数应用是左结合的，箭头是右结合的，于是

    $$f~a~b = (f~a)~b, A \to B \to C = A \to (B \to C)$$

3. 柯里化：一个有两个参数的函数，只是一个接收第一个参数，并返回“接收第二个参数的函数”的函数。例如$\lambda(a : A).\lambda(b : B).a$的类型为$A \to B \to A$，将其应用于$x : A$和$y : B$写作

    $$(\lambda(a : A).\lambda(b : B).a)~x~y = ((\lambda(a : A).\lambda(b : B).a)~x)~y$$

## 类型规则

判定"项$t$具有类型$A$"是由每种项的规则决定的。

### 上下文和判定

像是$f~p$这样的项不能被孤立地判定：它取决于$f$和$p$是什么。上下文$\Gamma$记录了这类信息。它是前提条件的一个有序列表

$$\Gamma = x_1 : A_1, x_2 : A_2, \dots, x_n : A_n$$

它给出了作用域中的每一个变量的类型。$dom(\Gamma)$表示变量名的集合$\{x_1, \dots, x_n\}$. 空上下文记作$\cdot$.

顺序是重要的，后面的类型可能依赖前面的变量，例如$P : Sort~0, h : P$. 这里的$h : P$只有当已知$P$是一个命题后才有意义。上下文中的变量名要求各不相同，所以上下文可以用作一个从变量名到类型的字典；只是这个字典的项可能依赖其前面的项。

我们先定义两个判定

- $\vdash\Gamma; ok$表示上下文$\Gamma$是良构的：每个类型都在上下文中提到它之前被定义了。
- $\Gamma\vdash t : A$表示在$\Gamma$给出的前提下，项$t$的类型是$A$

从Curry-Howard 同构的角度，$\Gamma\vdash t : A$表明在假设$\Gamma$的前提下，$t$是$A$的一个证明。

### 规则

1. 空上下文

    $$\frac{}{\vdash\cdot; ok}$$

2. 上下文扩展

    $$\frac{\vdash\Gamma; ok \quad \vdash A : Sort~u \quad x \notin dom(\Gamma)}{\vdash\Gamma, x : A; ok}$$

3. Sort类型

    $$\frac{\vdash\Gamma; ok}{\Gamma\vdash Sort~u : Sort~(u + 1)}$$

4. 变量类型

    $$\frac{\vdash\Gamma; ok \quad (x : A) \in \Gamma}{\Gamma\vdash x : A}$$

5. 依赖函数类型的类型

    $$\frac{\Gamma\vdash A : Sort~u \quad \Gamma, x : A \vdash B : Sort~v}{\Gamma\vdash\Pi(x : A).B : Sort~imax(u, v)}$$

    其中$imax(u, v) = 0$如果$v = 0$；否则$imax(u, v) = \max(u, v)$.
    换言之，只要依赖函数类型的主体是命题，它本身就是命题（不论它量化了什么）；否则它的类型就是两个宇宙中较大的那个。逻辑上，该规则说明了$A \to B$以及$\forall x : A. B$何时为良构的。

6. $\lambda$抽象的类型

    $$\frac{\Gamma\vdash A : Sort~u \quad \Gamma, x : A \vdash t : B}{\Gamma\vdash\lambda(x : A).t : \Pi(x : A).B}$$

7. 函数应用的类型

    $$\frac{\Gamma\vdash f : \Pi(x : A).B \quad \Gamma\vdash a : A}{\Gamma\vdash f~a : B[x \mapsto a]}$$

8. 类型的相等

    $$\frac{\Gamma\vdash t : A \quad \Gamma\vdash B : Sort~v \quad A \equiv B}{\Gamma\vdash t : B}$$

## 自由变量及替换

### 自由变量

- $FV(Sort~u) = \empty$
- $FV(x) = \{x\}$
- $FV(\Pi(x:A).B) = FV(A) \cup (FV(B) \setminus \{x\})$

    注意右边是$FV(A) \cup (FV(B) \setminus \{x\})$而不是$(FV(A) \cup FV(B)) \setminus \{x\}$，它们完全是两码事，下同。

- $FV(\lambda(x:A).t) = FV(A) \cup (FV(t) \setminus \{x\})$
- $FV(t~s) = FV(t) \cup FV(s)$

### 避免捕获的替换

1. $x[x\mapsto s] = s$
2. $y[x\mapsto s] = y \qquad (y \ne x)$
3. $Sort~u[x\mapsto s] = Sort~u$
4. $(t_1~t_2)[x\mapsto s] = t_1[x\mapsto s]~t_2[x\mapsto s]$
5. $(\square(x : A).B)[x\mapsto s] = \square(x : A[x\mapsto s]).B$
6. $(\square(y : A).B)[x\mapsto s] = \square(y : A[x\mapsto s]).B[x\mapsto s] \qquad (y \ne x, y \notin FV(s))$
7. $(\square(y : A).B)[x\mapsto s] = \square(z : A[x\mapsto s]).(B[y\mapsto z])[x\mapsto s] \qquad (y \ne x, y \in FV(s), z \notin FV(s) \cup FV(B) \cup \{x\})$

例子：

1. $(\Pi(h : P).P)[P\mapsto Q] = \Pi(h: Q).Q$ （情形6）
2. $(\lambda(x : A).x)[x\mapsto s] = \lambda(x : A).x$ （情形5）
3. $(\lambda(y : x).y)[x\mapsto s] = \lambda(y : s).y$ （情形6）
4. $(\lambda(y : A).x~y)[x \mapsto f~y] = \lambda(z : A).f~y~z$ （情形7）

## 归纳类型

### 常量与定义

在数理逻辑中，签名$\Sigma$是指一个代数结构包含哪些符号（包括常量、函数和关系）及其参数个数。$\Sigma$列出所有常量，每个条目要么只声明一个常量及其类型，写作$c : C$；要么还给出它的定义，写作$c : C := t$.

签名有两种用法。一是常量的类型规则：

$$\frac{\vdash\Gamma; ok \quad (c : C) \in \Sigma}{\Gamma\vdash c : C}$$

其中$(c : C) \in \Sigma$同时涵盖声明和定义。

二是定义的展开：用定义替换常量，称为$\delta$规约。

$$c \to_\delta t, \quad (c : C := t) \in \Sigma$$

### 递归

假设对任意自然数$n$，都存在某个函数$s_n : \mathbb N \to \mathbb N$，令$c$为某个固定的自然数。那么对任意自然数$n$，都能够确定唯一的自然数$f_n$，使得$f_0 = c$以及$f_{succ~n} = s_n(f_n)$恒成立。

函数$f$的值由两份数据决定：它在$0$上的值，即$c$；以及一种把它在$n$上的值变成它在$succ~n$上的值的方法，即$s_n$。每个用这种递归（称为结构递归）定义的函数$f : \mathbb N \to C$都具有相同的形状：

$$f~0 = z, \quad f~(succ~k) = s~k~(f~k)$$

其中$z : C, s : \mathbb N \to C \to C$，步骤函数$s$同时接收$k$和递归调用的结果$f~k$. 于是不妨一劳永逸地添加一个常量，它从$z$和$s$构造出$f$:

$$rec_C : C \to (\mathbb N \to C \to C) \to \mathbb N \to C$$

并配上两条计算规则，称为$\iota$规则，即前面两个等式：

$$rec_C~z~s~0 \to_\iota z, \quad rec_C~z~s~(succ~n) \to_\iota s~n~(rec_C~z~s~n)$$

例如，自然数的加法现在可以表示为

$$add : \mathbb N \to \mathbb N \to \mathbb N := \lambda(m : \mathbb N).\lambda(n : \mathbb N).rec_{\mathbb N}~m~s~n$$

其中$s = \lambda(k : \mathbb N).\lambda(r : \mathbb N).(succ~r)$

### 归纳

要证明性质$P~n$对每个$n$成立，其中$P : \mathbb N \to Sort~0$，数学上使用归纳法：证明**基础情形**$P~0$，以及**归纳步骤**$\Pi(k : \mathbb N).P~k \to P~(succ~k)$，后者的参数$P~k$是归纳假设。

根据Curry-Howard对应，$\Pi(n : \mathbb N).P~n$的证明是一个函数$f$，它接收$n$并返回$P~n$的证明。如何从基础情形$z : P~0$和归纳步骤$s : \Pi(k : \mathbb N).P~k \to P~(succ~k)$构造出这样的$f$？对于$3$，证明是$s~2~(s~1(s~0~base))$，一般地

$$f~0 = z, \quad f~(succ~k) = s~k~(f~k)$$

这就是前面的递归函数。**归纳就是返回证明的递归。**

区别在于结果类型：$add$对每个$n$都返回同一类型$C$，而证明的类型依赖于$n$，例如$f~0 : P~0, f~1 : P~1$。因此类型中带有固定$C$的$rec_C$还无法表达归纳。

| | 递归 | 归纳 |
| --- | --- | --- |
| $n$ 处的结果 | $C$ | $P~n$ |
| $0$ 情形 | $z : C$ | $z : P~0$ |
| $succ$ 情形 | $s : \Pi(k : \mathbb N).C \to C$ | $s : \Pi(k : \mathbb N).P~k \to P~(succ~k)$ |
| 结果 | $\mathbb N \to C$ | $\Pi(n : \mathbb N).P~n$ |

左列是右列的特例，其中$P~n$对每个$n$都是同一个类型$C$。所以我们采用右列，并把$P$换成任意函数$M : \mathbb N \to Sort~u$，它给出每个$n$处结果的类型，这个函数称为**motive**。所得就是下面的**递归子** (recursor) $\mathbb N.rec_u$，它的参数依次是motive，每个构造子对应的一个**小前提** (minor premise)，最后是要对其递归的数$n$，称为**大前提** (major premise)。它的$\iota$规则就是$rec_C$的那两条。

$$\begin{align*}
\mathbb N &: Sort~1 \\
0 &: \mathbb N \\
succ &: \mathbb N \to \mathbb N \\
\mathbb N.rec_u &: \Pi(M : \mathbb N \to Sort~u). M~0 \to (\Pi(n : \mathbb N). M~n \to M~(succ~n)) \to \Pi(n : \mathbb N). M~n
\end{align*}$$

$$\mathbb N.rec_u~M~z~s~0 \to_\iota z, \quad \mathbb N.rec_u~M~z~s~(succ~n) \to_\iota s~n~(\mathbb N.rec_u~M~z~s~n)$$

这一个递归子同时胜任两项工作，取决于motive。取$Sort~1$处的常值$\lambda(\_ : \mathbb N).C$，它就是递归；取$Sort~0$处的谓词$M : \mathbb N \to Sort~0$，它就是归纳原理。

$\mathbb N.rec_u$的类型并不是随意给的，它可以从Nat的声明中直接读出。回忆自然数类型的定义

``` lean
inductive Nat where
  | zero : Nat
  | succ (n : Nat) : Nat
```

每个构造子对应一个小前提，它接收该构造子的参数，并为类型是Nat本身的每个参数（递归参数）额外接收一个归纳假设，然后返回motive作用于该构造子的结果。
zero没有参数，所以它的情形就是$M~0$；succ有一个递归参数$n$，所以它的情形是$\Pi(n : \mathbb N). M~n \to M~(succ~n)$. 这套方法可以应用到越来越一般的声明上。

一般归纳类型的**递归子**类型如下：

$$I.rev_v : (M : I \to Sort~v) \to (m_1: C_1) \to \cdots (m_n : C_n) \to (x : I) \to M~x$$

考虑带参数和索引的归纳类型，完整的类型如下：

$$I.rev_v : \Pi(\vec p : \vec P).\Pi(M : \Pi(\vec i : \vec J). I~\vec p~\vec i \to Sort~v).\Pi(m_1 : C_1).\cdots\Pi(m_n:C_n).\Pi(\vec i : \vec J).\Pi(x : I~\vec p~\vec i).M~\vec i~x$$

其中小前提$C_j$要求构造子$c_j$的情形：给定它的字段$\vec a$，以及每个递归字段$a_k$的一个归纳假设，构造出$c_j$应用于这些字段时的$M$：

$$\begin{align*}
C_j &= \Pi(\vec a : \vec A_j).\Pi(\vec h : \vec H_j). M~\vec t_j~(c_j~\vec p~\vec a) \\
H_{j,k} &= \Pi(\vec y : \vec B). M~\vec s~(a_k~\vec y) \quad \forall a_k : \Pi(\vec y : \vec B). I~\vec p~\vec s
\end{align*}$$

最后的参数是索引和元素$x$本身，即大前提。

对每个构造子，内核生成一条$\iota$**规则**，它把小前提应用于字段和归纳假设，后者由递归调用算出：

$$I.rec_v~\vec p~M~\vec m~\vec i~ (c_j~\vec p'~\vec a) \to_\iota m_j~\vec a~\vec h, \quad h_k = \lambda(\vec y : \vec B).I.rev_v~\vec p~M~\vec m~\vec s~(a_k~\vec y)$$

大前提位于第$|\vec p| + 1 + n + |\vec i|$个位置（从零起），构造子有$|\vec p|$个参数和$|\vec a|$个字段。在良类型的可规约式中$\vec p'$可转换为$\vec p$，所以规则忽略它。这一过程恰好产生本章的递归子与规则。

### 接受条件

只有当以下四个条件都成立时，内核才接受一个声明。

1. $I$的类型在当前签名中是一个类型
2. 把$I$加入签名后，每个构造子类型都是一个类型，并且以$I~\vec p~\vec t_j$结尾，其中$\vec p$恰好是按顺序排列的参数，且$I$不出现在$\vec t_j$中。
3. **严格正性。** 每个字段类型要么不提及$I$，要么具有形式$\Pi(\vec y : \vec B).I~\vec p~\vec s$，其中$I$既不出现在$\vec B$中也不出现在$\vec s$中。第二种形式的字段称为**递归**的。
4. **宇宙。** 若$u \ge 1$， 每个字段类型都位于某个$Sort~w$中，且$w \le u$。若$u = 0$，则没有条件。
