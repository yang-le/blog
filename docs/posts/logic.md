---
date:
  created: 2024-11-20
categories:
    - 数学
---

# 数理逻辑简介

## 命题逻辑

\definition
    一个命题是一个只能取真（T）或假（F）变量。

    永真的命题称为重言式（tautology），永假的命题称为矛盾式（contradiction）。

<!-- more -->

我们可以使用逻辑算符从给定的命题构建新的命题。最简单的一种逻辑算符称为一元算符，共四种，它们是

| $p$ | $\neg p$ | $\mathrm{id}(p)$ | $\top p$ | $\bot p$ |
|--|--|--|--|--|
| F | T | F | T | F |
| T | F | T | T | F |

下一步是考虑二元算符，共十六种。以下我们列举常见的几种

| $p$ | $q$ | $p \land q$ | $p \lor q$ | $p \veebar q$ | $p \Rightarrow q$ | $p \Leftrightarrow q$ | $p \uparrow q$ |
|--|--|--|--|--|--|--|--|
| F | F | F | F | F | T | T | T |
| F | T | F | T | T | T | F | T |
| T | F | F | T | T | F | F | T |
| T | T | T | T | F | T | T | F |

\theorem
    设$p, q$是命题，则$(p \Rightarrow q) \Leftrightarrow ((\neg q) \Rightarrow (\neg p))$.

\proof
    如下表所示

    | $p$ | $q$ | $\neg p$ | $\neg q$ | $p \Rightarrow q$ | $(\neg q) \Rightarrow (\neg p)$ |
    |--|--|--|--|--|--|
    | F | F | T | T | T | T |
    | F | T | T | F | T | T |
    | T | F | F | T | F | F |
    | T | T | F | F | T | T |

    注意最后两列是相同的，因此定理得证。

我们规定常用算符的优先级如下：

$$\neg, \land, \lor, \Rightarrow, \Leftrightarrow.$$

这将使我们免去不必要的括号，例如$(\neg q) \Rightarrow (\neg p)$现在可以写为$\neg q \Rightarrow \neg p$.

现在考虑更高阶的算符，如三元、四元等等。可以证明，所有这些高阶算符都可以由一个单一的二元算符构造出来，这个二元算符$\uparrow$称为与非算符，我们有$(p \uparrow q) \Leftrightarrow \neg(p \land q)$，见上表。

至此，我们对逻辑算符的讨论就可告一段落了。

## 谓词逻辑

\definition
    一个谓词是一个（关于某个或某些变量的）函数，其值为命题。注意这里我们不限制这些输入变量的取值。

    一个关于两个变量的谓词称为关系。

### 全称量词

\definition
    设$P(x)$是一个谓词，则

    $$\forall x: P(x)$$

    也是一个谓词。我们定义它为真，若不论$x$是何取值，$P(x)$总是为真；否则我们定义它为假。这里的符号$\forall$称为全称量词。

### 存在量词

\definition
    设$P(x)$是一个谓词，我们定义

    $$\exist x : P(x)$$

    为$\neg(\forall x: \neg P(x))$. 显然它也是一个谓词。这里的符号$\exist$称为存在量词。

\theorem
    设$P(x)$是一个谓词，则

    $$\forall x : P(x) \Leftrightarrow \neg(\exist x : \neg P(x))$$

我们可以给多于一个变量的谓词定义量词，只要我们分步骤为各变量施加量词即可。例如，设$P(x, y)$是一个谓词。那么对于固定的$y$，$P(x, y)$可以看作是一个变量的谓词，我们定义$Q(y)$为

$$\forall x : P(x, y)$$

于是$\exist y : Q(y)$就是

$$\exist y : \forall x : P(x, y)$$

注意量词的顺序是重要的，例如上式并不一定等同于

$$\forall x : \exist y : P(x, y)$$

后者的$y$是可以依赖于$x$的。

## 公理系统和证明论

\definition
    一个公理系统是一个有限的命题序列$a_1, a_2, \dots, a_N$，它们称为这个系统的公理。

    我们称一个有限的命题序列$q_1, q_2, \dots, q_M$为命题$p$在一个公理系统$a_1, a_2, \dots, a_N$中的一个证明，若$q_M = p$且任意的$q_j, 1 \le j \le M$满足以下三个条件之一
    
    - $q_j$是该公理系统中的公理之一
    - $q_j$是一个重言式
    - $\exist 1 \le m, n \le j : (q_m \land q_n) \Rightarrow q_j$

    若这样的序列$q_1, q_2, \dots, q_M$存在，我们就说$p$在公理系统$a_1, a_2, \dots, a_N$中可被证明，记作
    
    $$a_1, a_2, \dots, a_N \vdash p$$
    
    读作$a_1, a_2, \dots, a_N$证明了$p$.

从上述定义我们可以看出，任何出现在公理系统中的重言式都可以从中移除而不损害该公理系统的威力。公理系统的一个极端的例子是命题逻辑，该情形下的公理系统是空序列。这意味着任何我们可在命题逻辑中证明的东西都只不过是重言式。

### 公理系统的相容性

\definition
    我们称一个公理系统$a_1, a_2, \dots, a_N$是相容的，若存在不可由该公理系统证明的命题。以符号表示为
    
    $$\exist q : \neg(a_1, a_2, \dots, a_N \vdash q)$$

让我们考虑一个包含矛盾的公理系统

$$a_1, \dots, s, \dots, \neg s, \dots, a_N$$

则对于任意的$q$，以下是$q$在该系统中的一个证明

$$s, \neg s, q$$

实际上，$s$和$\neg s$是该证明中的合法步骤，因为它们是公理。而$s \land  \neg s$是矛盾式，因此$(s \land  \neg s) \Rightarrow q$是重言式。这表明任何命题都可由包含矛盾公理的系统证明。换言之，不能证明所有命题是无矛盾公理系统的一个性质，因此我们定义相容的公理系统为具有这种性质的系统。

\theorem
    命题逻辑是相容的。

\proof
    我们只需说明存在命题逻辑无法证明的命题即可。命题逻辑的公理系统为空序列。因此我们的证明只能是这样的序列，其中$q_j$满足以下二者之一

    - $q_j$是一个重言式
    - $\exist 1 \le m, n \le j : (q_m \land q_n) \Rightarrow q_j$

    后者允许我们插入这样的命题$q_j$，使得$(q_m \land q_n) \Rightarrow q_j$为真，其中$q_m$和$q_n$是证明序列中先于$q_j$的命题。但是由于第一个条件只允许我们在证明序列中插入重言式，命题$q_m$和$q_n$必须为重言式。于是为了使得$(q_m \land q_n) \Rightarrow q_j$为真，$q_j$也必须是重言式。于是证明序列全部由重言式组成，而且因此只有重言式可被证明。

    现在设$q$为任意命题，则$q \land \neg q$是一个矛盾式。由于它不是一个重言式，因此不能被证明。根据定义，这就说明命题逻辑是相容的。

实际上，证明给定公理系统的相容性是很难的，但命题逻辑是一个例外。

\theorem
    哥德尔第一不完备定理

    任何蕴含皮亚诺算术公理的公理系统要么是不相容的，要么包含一个不可判定的命题，即存在一个命题既不能由该系统证明，也不能由该系统证否。

连续统假设就是不可判定命题的一个例子，在集合论的ZF公理系统中，它既不能被证明，也不能被证否。
