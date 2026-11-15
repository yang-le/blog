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
