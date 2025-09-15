---
date:
  created: 2024-11-21
categories:
    - 数学
---

# ZFC集合论

## $\in$-关系

集合论构建于这样的假设之上，即有一个基本的关系（即一个二变量的谓词），记作$\in$. 集合论并不直接定义$\in$是什么，也不直接定义集合是什么，而是给出了关于$\in$和集合的九个公理，也正是这九个公理完整地定义了$\in$和集合。

<!-- more -->

## 集合论的Zermelo-Fraenkel公理

### 外延公理

$$\forall x: \forall y: (\forall z: (z \in x \Leftrightarrow z \in y) \Rightarrow \forall w: (x \in w \Leftrightarrow y \in w))$$

我们定义$x = y$为

$$\forall z: (z \in x \Leftrightarrow z \in y) \land \forall w: (x \in w \Leftrightarrow y \in w)$$

于是外延公理也可以写为

$$\forall x: \forall y: (\forall z: (z \in x \Leftrightarrow z \in y) \Rightarrow x = y)$$

意为两个集合相等（是同一集合），若它们有相同的元素。

### 空集公理

$$\exist x: \neg\exist y: y \in x$$

或以$\notin$符号表示为

$$\exist x: \forall y: y \notin x$$

意为存在这样的集合，它不包含任何元素。结合外延公理，我们可以证明，这样的集合是唯一的，称之为空集，记作$\empty$.

\theorem
    空集是唯一的。

\proof
    设$x$和$x'$都是空集，则$y \in x$为假，这就意味着$y \in x \Rightarrow y \in x'$为真。注意到这个结论是不依赖于$y$的，即

    $$\forall y: y \in x \Rightarrow y \in x'$$

    类似地，我们有

    $$\forall y: y \in x' \Rightarrow y \in x$$

    即

    $$\forall y: y \in x \Leftrightarrow y \in x'$$

    根据外延公理，我们就有$x = x'$.

### 配对公理

$$\forall x: \forall y: \exist z: \forall u: (u \in z \Leftrightarrow (u = x \lor u = y))$$

意为对于任意的$x$和$y$，存在一个集合$z$恰以$x$和$y$为其元素，我们记作$\{x, y\}$. 根据定义，我们有$\{x, y\} = \{y, x\}$，于是这个对是无序的。同样可以使用外延公理证明，这样的集合也是唯一的，我们称之为$x$和$y$的（无序）对。

我们将集合$\{x, x\}$简写为$\{x\}$，称为包含$x$的单元素集。

配对公理还允许我们定义$(x, y) := \{\{x\}, \{x, y\}\}$，称为$x$和$y$的序对。注意这个定义满足条件

$$(a, b) = (c, d) \Leftrightarrow a = c \land b = d$$

有序$n$-元组可被递归地定义如下：

$$(a_1, \dots, a_n) := ((a_1, \dots, a_{n - 1}), a_n)$$

> 配对公理并非独立于其他公理。实际上，它可由替换公理模式应用于任给的二元素或多元素集合得到。而这样一个二元素集合（例如$\{\empty, \{\empty\}\}$）的存在性，可由空集公理和幂集公理或无穷公理得到。

### 并集公理

$$\forall x: \exist u: \forall y: (y \in u \Leftrightarrow \exist s : (y \in s \land s \in x))$$

对任意的集合$x$，都存在一个集合$u$，它恰以集合$x$的元素($s$)的元素($y$)为其元素。记作$u = \bigcup x$.

### 替换公理模式

$$\forall m: (\forall x: \exist!y: x \in m \Rightarrow R(x, y)) \Rightarrow \exist n: \forall y: (y \in n \Leftrightarrow \exist x: (x \in m \land R(x, y)))$$

我们称关系$R$是一个泛函关系，若$\forall x: \exist!y : R(x, y)$. 其中$\exist!$表示存在且唯一。设$m$是一个集合，$m$在$R$下的像由所有满足$\exist x: (x \in m \land R(x, y))$的那些$y$构成。记作$\mathrm{im}_R(m)$.
替换公理模式保证这个像是一个合法的集合。

替换公理模式可以简单地记为

$$\forall m: \exist n: \forall y: (y \in n \Leftrightarrow \exist x: (x \in m \land R(x, y)))$$

其中$R$是一个泛函关系。所以替换公理模式并不是一个公理，而是依赖于$R$的一族公理，因此称为公理模式。

\theorem
    设$P(x)$是一个谓词，$m$是一个集合，则所有使得$P(y)$为真的$y \in m$构成一个集合，记为
    
    $$\{y \in m | P(y)\}$$

\proof
    为了简单起见，我们将

    $$\forall x \in y: P(x)$$

    定义为

    $$\forall x: (x \in y \Rightarrow P(x))$$

    并将

    $$\exist x \in y: P(x)$$

    定义为

    $$\exist x: (x \in y \land P(x))$$

    证明分两种情况：

    1. 若$\neg(\exist y \in m: P(y))$，我们定义$\{y \in m | P(y)\}$为$\empty$，根据空集公理，它是一个集合。
    2. 若$\exist\hat y \in m : P(\hat y)$，则令$R$为如下的泛函关系

    $$R(x, y) \Leftrightarrow (P(x) \land x = y) \lor (\neg P(x) \land \hat y = y) $$

    换言之，若$P(x)$为真，$R$将选出$x$；否则选出$\hat y$。于是$\{y \in m | P(y)\}$就是$\mathrm{im}_R(m)$，根据替换公理模式，它是一个集合，证毕。

该定理被称为受限概括原理（公理模式），Principle of Restricted Comprehension（PRC），分类（分离）公理模式。
某些版本的ZFC不包含空集公理，而是使用该分离公理模式来定义空集。

请勿将此和无限制概括原理（Principle of Universal Comprehension）混淆，后者声称对任意谓词$P, \{y | P(y)\}$是一个集合，而这被罗素证明是（逻辑上）不相容的。
注意其中的$y \in m$使得集合$\{y \in m | P(y)\}$所含的元素不会超过$m$本身。

现在我们可以定义集合的交$\bigcap x$为

$$\{a \in \bigcup x | \forall b \in x : a \in b\}$$

若$a, b \in x$且$\bigcap x = \empty$，则称$a$和$b$是不交的。

定义$x \sube y$为$\forall a \in x: a \in y$，现在设$u$和$m$是集合，且满足$u \sube m$，我们定义$u$相对于$m$的补$m \setminus u$为

$$\{x \in m | x \notin  u\}$$

此外若$x \sube y$且$x \neq y$我们就记$x \sub y$.

### 幂集公理

$$\forall x: \exist y: \forall a: (a \in y \Leftrightarrow a \sube x)$$

设$x$为一集合，则存在一个集合$y$，它恰以$x$的子集为其元素，称为$x$的幂集，记作$\mathcal P(x)$.

历史上曾将幂集定义为$\{y | y \sube m\}$，为了以这种风格定义幂集，我们必须事先说明$y$所在的集合以将其限制。
但是，在仅基于前述公理的限定下这是不可能的；实际上，我们别无他法而只能为幂集的存在性专门增加一个公理。

### 无穷公理

$$\exist x: \empty \in x \land (\forall y \in x: y \cup \{y\} \in x)$$

存在这样一个集合$x$，它包含空集作为其元素，且若$y$是它的元素，则$y \cup \{y\}$也必是它的元素。
这样的集合有时也叫做归纳集合，其中$y \cup \{y\}$叫做$y$的后继。无穷公理允许这样的集合存在。

这个集合$x$实际上就是自然数集$\N$，所以无穷公理也相当于是说存在这样一个集合，它包含所有的自然数。
此外，我们还可以定义$\R$为$\mathcal P(\N)$，可以证明它和实数集是（集合）同构的。
无穷公理和幂集公理保证了自然数集和实数集的存在性。

> 注意无穷公理和并集公理的独立性。将所有后继取并不能得到自然数集。

### 正则公理

$$\forall x: (\exist a: (a \in x) \Rightarrow \exist y: (y \in x \land \neg\exist z: (z \in y \land z\in x)))$$

每个非空集合$x$都包含一个元素$y$，满足$y$与$x$不相交。以现代的记号写就是

$$\forall x: (x \ne \empty \Rightarrow \exist y: (y \in x \land y \cap x = \empty))$$

这个公理也称基础公理。它的一个应用是用来证明满足$x \in x$这样的集合是不存在的。若我们假设存在这样的集合，则根据配对公理我们有$\{x\}$也是一个集合，于是根据$x \in \{x\}$和$x \in x$我们就有$x \cap \{x\} = x$. 而对$\{x\}$使用正则公理我们应该有$x \cap \{x\} = \empty$. 但根据我们的假设$x$为非空，这就导致了矛盾。

### 选择公理

$$\forall x: ((\empty \notin x \land \forall a, b \in x: a \cap b = \empty) \Rightarrow \exist y: \forall a\in x: \exist!b \in a: b \in y)$$

设$x$的元素为两两互不相交的非空集合，则存在一个集合$y$，它恰好包含$x$中的每个元素（集合）的一个元素。
换言之，存在一个从$x$到$\bigcup x$的映射$f$，满足$\forall a \in x: f(a) \in a$. 而$y$则定义为$x$在这个映射下的像$f[x]$。
这个映射被称为选择函数，选择公理声明这样的选择函数是存在的。

## 集合的分类

### 集合间的映射

数学中一个反复出现的主题是通过各种空间之间保结构的映射来对空间进行分类。这里的空间是指装备了某种结构的集合，而结构通常是某种其他的集合。对于将集合本身看作空间来说，就没有其它的结构了，或者说结构就是空集。

设$A, B$是集合。一个映射$\phi: A \to B$是一个关系，使得对每一个$a \in A$恰存在一个$b \in B$满足$\phi(a, b)$. 通常这个唯一的$b$记作$\phi(a)$.

对于映射$\phi: A \to B$，我们有以下的标准术语

- 集合$A$称为$\phi$的定义域
- 集合$B$称为$\phi$的值域
- 集合$\phi(A) \equiv \mathrm{im}_\phi(A) := \{\phi(a) | a \in A\}$称为$A$在$\phi$下的像

称映射$\phi: A \to B$为

- 入射，如果$\forall a_1, a_2 \in A: \phi(a_1) = \phi(a_2) \Rightarrow a_1 = a_2$
- 满射，如果$\mathrm{im}_\phi(A) = B$
- 双射，如果它既是入射也是满射

称两个集合$A, B$是同构的，若存在双射$\phi: A \to B$. 这里“保结构”的映射就是双射。直观上，双射把$A$和$B$中的元素进行了配对，且$A, B$之间存在双射仅当它们有相同的“大小”。对于有限集合来说，这是清楚的；但这个想法可以扩展到无限集合。

一个集合$A$称为是

- 无限的，如果存在$A$的一个真子集$B \sub A$，满足$B$和$A$同构。特别地，若$A$是无限的，我们进一步地称$A$为
    - 可数无穷的，如果$A$同构于$\N$
    - 否则称为不可数无穷的
- 有限的，如果它不是无限的。在这种情况下我们有$A$同构于$\{1, 2, \dots, N\}$其中$N$是某个自然数，我们称$A$的基数，记为$|A|$，是$N$.

给定两个映射$\phi: A \to B$和$\psi: B \to C$，我们可以构造第三个映射，称为$\phi$和$\psi$的复合，记作$\psi \circ \phi$，定义为

$$\psi \circ \phi: A \to C$$

其中$(\psi \circ \phi)(a) := \psi(\phi(a))$. 这经常表示为下面的图

\tikzcd
    & B \arrow[rd, "\psi"] & \\
    A \arrow[ru, "\phi"] \arrow[rr, "\psi\circ\phi"'] & & C

并称此图是交换的，意思是图中的每条路径给出相同的结果。以后我们会遇到有很多映射的情形，这些图可以极大地简化我们的说明。

可以证明映射的复合满足结合律，即$\xi \circ (\psi \circ \phi) = (\xi \circ \psi) \circ \phi$.

设$\phi: A \to B$是一个双射，$\phi$的逆映射，记作$\phi^{-1}$，定义为

$$\phi^{-1}\circ\phi = \mathrm{id}_A \\
\phi\circ\phi^{-1} = \mathrm{id}_B$$

等价地，我们要求下图交换：

\tikzcd
    A \arrow[r, "\phi", bend left] \arrow["\mathrm{id}_A"', loop, distance=2em, in=215, out=145] & B \arrow[l, "\phi^{-1}", bend left] \arrow["\mathrm{id}_B"', loop, distance=2em, in=35, out=325]

逆映射只对双射有定义。但我们将经常在拓扑中见到的如下记号，则对任意映射都有定义。

设$\phi: A \to B$是一个映射且$V \sube B$. 我们定义集合

$$\mathrm{preim}_\phi(V) := \{a \in A | \phi(a) \in V\}$$

称为$V$在$\phi$下的原像。

### 等价关系

设$M$是一个集合且$\sim$是一个满足如下条件的关系：

- 自反性：$\forall m \in M: m \sim m$
- 对称性：$\forall m, n \in M: m \sim n \Leftrightarrow n \sim m$
- 传递性：$\forall m, n, p \in M: (m \sim n \land n \sim p) \Rightarrow m \sim p$

则$\sim$称为一个$M$上的等价关系。若$\sim$为一个$M$上的等价关系，则对任意的$m \in M$，我们定义集合

$$[m] := \{n \in M | m \sim n\}$$

称为$m$的等价类。关于等价类，我们有如下的两条关键性质：

- $a \in [m] \Rightarrow [a] = [m]$
- 要么$[m] = [n]$，要么$[m] \cap [n] = \empty$

设$\sim$为一个$M$上的等价关系，我们定义$M$关于$\sim$的商集为：

$$M / \sim := \{[m] \in \mathcal P(M) | m \in M\}$$

### $\N, \Z, \mathbb Q$和$\R$的构造

回忆前面，通过无穷公理，我们定义

$$\N := \{0, 1, 2, 3, \dots\}$$

其中

$$0 := \empty, 1 := \{\empty\}, 2 := \{\empty, \{\empty\}\}, 3 := \{\empty, \{\empty\}, \{\empty, \{\empty\}\} \}, \dots$$

为了定义加法，我们需要几个基础的定义。

$\N$上的后继映射$S: \N \to \N$定义为

$$S(n) := n \cup \{n\}$$

$N^*$上的前驱映射$P: \N^* \to N$定义为

$$P(n) := \bigcup n$$

这里$N^*$表示$\N \setminus \{0\}$. 此外，我们递归地定义$S^n$为：

$$\begin{align*}S^n &:= S \circ S^{P(n)}, ~ n \in N^*\\
S^0 &:= \mathrm{id}_\N\end{align*}$$

现在我们可以定义加法。$\N$上的加法操作是一个映射$+:\N \times \N \to \N$，定义为

$$m + n := S^n(m)$$

使用这个定义，我们可以验证加法满足交换律和结合律。加法的中性元是$0$。显然，$\N$中不存在加法的逆元，即给定一个非零的$m \in \N$，不存在这样的$n \in \N$使得$m + n = 0$. 这是扩展自然数到整数的动机。为了定义整数$\Z$，我们需要先定义如下的关系。

令$\sim$是$\N \times \N$上的关系，其定义为

$$(m, n) \sim (p, q) \Leftrightarrow m + q = p + n$$

容易验证，这是一个等价关系。我们定义整数的集合为

$$\Z := (\N \times \N) / \sim$$

这个定义背后的想法是序对$(m, n)$表示“$m - n$”. 换言之，我们用自然数的差（尚待定义）来表示整数。显然，有多种方式来表示同一个整数。例如，整数$-1$可以表示为$(1, 2), (2, 3), (112, 113), \dots$

但是请注意$(1, 2) \sim (2, 3), (1, 2) \sim (112, 113)$等等。实际上，取关于$\sim$的商集处理了这种“冗余”。也请注意，这里我们不说$\N \sube \Z$，毕竟$\N$和$\Z := (\N \times \N) / \sim$包含完全不同的元素，说它们中的一个是另一个的子集是完全没有意义的。然而$\N$确实可以嵌入到$\Z$，即存在一个包含映射$\iota : \N \hookrightarrow \Z$，定义为

$$\iota(n) := [(n, 0)]$$

在这个意义上我们说$\N$是包含于$\Z$的。

令$n := [(n, 0)] \in \Z$，我们定义$n$的逆元为$-n := [(0, n)]$. 可以从$\N$将加法的定义继承过来，我们定义整数的加法$+_\Z: \Z \times \Z \to \Z$为：

$$[(m, n)] +_\Z [(p, q)] := [(m + p, n + q)]$$

类似地，我们定义有理数的集合为

$$\mathbb Q := (\Z \times \Z^*) / \sim$$

其中$\sim$是$\Z \times \Z^*$上的关系：

$$(p, q) \sim (r, s) \Leftrightarrow ps = qr$$

这里假定整数上已经定义了乘法操作。和整数类似的情况，我们有一个从$\Z$到$\mathbb Q$的正则嵌入$\iota : \Z \hookrightarrow \mathbb Q$，定义为

$$\iota(p) := [(p, 1)]$$

定义有理数的加法$+_{\mathbb Q} : \mathbb Q \times \mathbb Q \to \mathbb Q$为：

$$[(p, q)] +_{\mathbb Q} [(r, s)] := [(ps + rq, qs)]$$

定义有理数的乘法$\cdot_{\mathbb Q} : \mathbb Q \times \mathbb Q \to \mathbb Q$为：

$$[(p, q)] \cdot_{\mathbb Q} [(r, s)] := [(pr, qs)]$$

有许多方法可以从有理数构造实数。其中之一是定义集合$\mathscr A$为$\Z$上的殆同态的集合，然后定义$\R$为：

$$\R := \mathscr A / \sim$$

其中$\sim$是$\mathscr A$上的一个“适当”的等价关系。详见[R.D. Arthan. The Eudoxus Real Numbers](https://arxiv.org/abs/math/0405454)和[Norbert A'Campo. A natural construction for the real numbers](https://arxiv.org/abs/math/0301015).
