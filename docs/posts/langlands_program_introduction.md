---
date:
  created: 2024-11-12
categories:
    - 数学
---

# Langlands纲领介绍

<aside>本文为b站视频<a href="https://www.bilibili.com/video/BV1sv411r7tc">莫仲鹏教授：Langlands纲领介绍</a>的笔记整理，仅作个人学习之用。若相关作者觉得有不当之处，可随时私信我处理。</aside>

<!-- more -->

## 记号

|符号| 说明 |
|--|--|
| $\Z$ | 正整数集合，$n \in \Z, n \ge 1$ |
| $(a, n)$ | $a, n$的最大公约数，$a, n$互素则$(a, n) = 1$ |
| $\Z / n\Z$ | 模$n$同余环 |
| $a \equiv b \mod n$ |  $a, b$模$n$同余，$n \mid a - b$ |
| $\mathbb F_p$ |  当$n= p$为素数时，$\Z/p\Z$是有限域，记作$\mathbb F_p$  |

## 二次同余方程

固定$d\in \Z$，$d$不是平方数，考虑同余方程、

$$x^2 \equiv d \mod p$$

其中$p$是素数，且$p \nmid 2d$。设$N_p$为该同余方程在有限域$\mathbb F_p$上的解的个数。

$$N_p = \#\{x \mod p \mid x^2 \equiv d \mod p\}$$

则有$N_p = 0$或$2$。我们有

$$N_p = 1 + \left(d \over p\right)$$

其中$\left(d \over p\right)$为Legendre符号

$$\left(d \over p\right) = \left\{\begin{aligned}
+1, & \text{如果}x^2 \equiv d \mod p \text{有解}\\
-1, & \text{如果}x^2 \equiv d \mod p \text{无解}
\end{aligned}\right.$$
!!! question "问题"
    随着$p$的变动，$N_p$的规律是什么？

利用二次互反律(Gauss，1796)可以得到如下：
\theorem
    对于固定的$d \in \Z$，$d$不是平方数，则$N_p$($p$是素数且$p\nmid 2d$)只依赖于$p$的模$4|d|$同余类。

也就是说如果$p_1 \equiv p_2 \mod 4|d|$，则$N_{p_1} = N_{p_2}$。而且存在同态(即对于所有的$m, n \mod 4|d|$,有$\chi(mn) = \chi(m)\chi(n)$)

$$\chi\colon(\Z / 4|d|\Z)^\times \to \{\pm1\}$$

使得对于素数$p, p\nmid 2d$，有

$$\left(d \over p\right) = \chi(p)$$

???+ note "注"
    一般的模$N$Dirichlet特征定义为同态

    $$\chi\colon(\Z / N\Z)^\times \to \mathbb C^\times$$

    其中$\chi$的取值为单位根。 因为$d$不是平方数，所以多项式$x^2 - d$在$\mathbb Q$上不可约，我们有
    
    $$\operatorname{Gal}(\mathbb Q(\sqrt d) / \mathbb Q) \cong\Z / 2\Z \cong \{\pm 1\}$$

    是交换群。

## 三次同余方程

考虑三次多项式

$$x^3 - x - 1$$

其在$\mathbb Q$上为不可约，椭圆曲线判别式$\Delta = -(4a^3 + 27b^2) = -23$。
对于素数$p \ne 23$，定义

$$N_p = \#\{x \mod p \mid x^3 - x - 1 \equiv 0 \mod p\}$$

有$N_p = 0, 1$或$3$。

!!! question "问题"
    随着$p$的变动，$N_p$的规律是什么？

可以证明，不存在之前那样的只依赖于$p$的同余类就可以描述$N_p$的规律。对于$p \ne 2, 23$有:

$$N_p= \left\{\begin{aligned}
0\text{或}3, & \text{如果}\left(-23 \over p\right) = 1\\
1, & \text{如果}\left(-23 \over p\right) = -1
\end{aligned}\right.$$

???+ note "注"
    二次互反律指出，对于
    $p \ne 2, 23, \left(-23 \over p\right) = \left(p \over23\right)$。
    所以条件$\left(-23 \over p\right)$只依赖于$p \mod 23$，
    但没有办法只通过$p$的同余类来判断$N_p = 0$还是$3$。

我们有如下(Hecke, 1920's)
\theorem
    考虑

    $$f(z) = q\prod_{n = 1}^\infty(1-q^n)(1-q^{23n})$$

    这里

    $$q = e^{2\pi iz}$$

    其中$z \in \mathcal H = \{z \in \mathbb C \mid \operatorname{Im} z 0\}$, $f$在$\mathcal H$上全纯，且可以写成
    
    $$f(z) = \sum_{n = 1}^\infty a_nq^n$$

    $a_1 = 1, a_n$为常数。则对于素数$p \ne 23$，有
    
    $$N_p = 1 +a_p$$

$f(z)$的重要性在于，$f$是模形式。更准确地说，$f$是权为1，级为23，带特征$\left(\cdot \over 23\right)$的尖点模形式。也就是说，对于所有的$\begin{pmatrix}a & b \\ c & d\end{pmatrix} \in \Gamma_0(23)$，以及$z \in \mathcal H$，有

$$f(\frac{az + b}{cz + d}) = \left(d \over 23\right)\cdot(cz + d)\cdot f(z)$$

对于一般的$N \ge 1$，$\Gamma_0(N)$的定义为：

$$\Gamma_0(N) = \{\begin{pmatrix}a & b \\ c & d\end{pmatrix} \in \operatorname{SL}_2(\Z) \mid c \equiv 0 \mod N\}$$

设$S$为多项式$x^3 - x - 1$在$\mathbb Q$上的分裂扩张，则有

$$\operatorname{Gal}(S/\mathbb Q) \cong S_3$$

为置换群，特别地，还是非交换群。这是和之前的二次同余的最重要的区别。

???+ note "注"
    设

    $$\rho \colon S_3 \to \operatorname{GL}_2(\mathbb C)$$
    
    为$S_3$的(唯一)二维不可约表示,则$\operatorname{tr}\rho$的取值就是$-1, 0, 2$。
    利用代数数论方法可以对$p \ne 23$定义在$\operatorname{Gal}(S/\mathbb Q) \cong S_3$
    中的Frobenius共轭类$\operatorname{Frob}_p$，使得有： 
    
    $$N_p = 1 + \operatorname{tr}\rho(\operatorname{Frob}_p)$$
    
    所以上述定理其实就是说，对于素数$p \ne 23$，有
    
    $$\operatorname{tr}\rho(\operatorname{Frob}_p) = a_p$$
    
    对于一般的在$\mathbb Q$上的不可约的三次方程，有类似的结论(Hecke, Maass 1920's - 1940's)。


???+ note "注"
    回到之前的二次同余的情况，多项式为
    
    $$x^2 - d$$

    $d$不是平方数。一维表示
    
    $$\rho \colon \operatorname{Gal}(\mathbb Q(\sqrt d)/\mathbb Q) \cong \{\pm 1\} \sub \mathbb C^\times = \operatorname{GL}_1(\mathbb C)$$
    
    则对于素数$p \nmid 2d$，可以定义元素
    
    $$\operatorname{Frob}_p \in \operatorname{Gal}(\mathbb Q(\sqrt d)/\mathbb Q)$$
    
    使得
    
    $$\operatorname{tr}\rho(\operatorname{Frob}_p) = \rho(\operatorname{Frob}_p) = a_p$$
    
    同样有
    
    $$N_p = 1 + \rho(\operatorname{Frob}_p)$$
    
    这样，前面由二次互反律给出的推论其实就是说，存在Dirichlet特征
    
    $$\chi \colon (\Z/4|d|\Z)^\times \to \{\pm 1\} \sub \mathbb C^\times$$
    
    使得对于素数$p \nmid 2d$，有
    
    $$\rho(\operatorname{Frob}_p) = \chi(p)$$

!!! abstract "小结"
    概括地说，我们希望把二次互反律的结果推广至非交换的Galios扩张。Langlands认为，在非交换的情形下，Dirichlet特征应该被取代为模形式，或者是更加一般的自守形式（或自守表示）。对于交换的Galios扩张的情形，类域论已经给出了完整的结果。

## 自守形式

对于$SL_2$或$GL_2$，其自守形式分为两种：

### 全纯模形式

设$k \ge 1， N \ge 1, \chi$为模$N$的Dirichlet特征。权$k$，级$N$带特征$\chi$的全纯尖点模形式$f$为$\mathcal H$上的全纯函数。并且对于$\begin{pmatrix}a & b \\ c & d\end{pmatrix} \in \Gamma_0(N)$有

$$f(\frac{az + b}{cz + d}) = \chi(d)\cdot(cz + d)^k\cdot f(z) ~ \forall z \in \mathcal H$$

且在尖点上的取值为0。特别地，$f(z + 1) = f(z)$，且该函数存在$q$-展开。记$S_k(N, \chi)$为权$k$，级$N$，带特征$\chi$的全纯尖点模形式的$\mathbb C$-向量空间，我们有

$$\dim_{\mathbb C}S_k(N, \chi) < \infty$$

### Maass模形式

设$N \ge 1, \chi$为模$N$的特征。$f$在$\mathcal H$上为实解析的$\mathbb C$-值函数。并且对于$\begin{pmatrix}a & b \\ c & d\end{pmatrix} \in \Gamma_0(N)$有

$$f(\frac{az + b}{cz + d}) = \chi(d)\cdot f(z) ~ \forall z \in \mathcal H$$

且$f$是双曲Laplace算子

$$\Delta = -y^2(\frac{\partial^2}{\partial x^2} + \frac{\partial^2}{\partial y^2})$$

的本征函数。即

$$\Delta f = \lambda\cdot f$$

其中$\lambda > 0$是本征值。$\lambda$通常写成

$$\lambda = \frac{1}{4} + r^2 = \frac{1}{4} - (ir)^2$$

其中$ir \in i\R \cup (-\frac{1}{2}, \frac{1}{2})$。具体属于哪个区间则要看$\lambda \ge \frac{1}{4}$还是$0 < \lambda < \frac{1}{4}$。且这里仍要求$f$在尖点上的取值为$0$。
对于$N, \chi, \lambda$，这也构成一个$\mathbb C$-向量空间$S_\lambda^M(N, \chi)$，且有

$$\dim_{\mathbb C}S_\lambda^M(N, \chi) < \infty$$

对于Maass尖点模形式，有Fourier展开

$$f(x + iy) = \sum_{n \ne 0}a_n\sqrt yK(2\pi|n|y;ir)\cdot e^{2\pi inx}$$

其中$a_n$为常数，$K$是参量为$ir$的K-Bessel函数

$$K(y;ir) = \frac{1}{2}\int_{-\infty}^\infty e^{-y\operatorname{cosh}t - irt}dt$$

且$y > 0$。

关于$\lambda$，有Selberg猜想：$\lambda \ge \frac{1}{4}$（即$ir \in i\R$）。
Selberg自己证明了(1965)：$\lambda \ge \frac{3}{16}$，且$N = 1$时，有$\lambda > \frac{1}{4}$。
目前最好的结果是Kim-Sarnak(2003)：$\lambda \ge \frac{1}{4} - (\frac{7}{64})^2$。

???+ note "注"
    Kim-Sarnak关于Selberg猜想的结果其中关键的一步是Kim-Shahidi(2002)关于Langlands函子性猜想的一些特殊情形。在Kim-Shahidi中，需要用到例外群$E_7$的自守形式和自守表示理论。

## Hecke本征形式

在有限维$\mathbb C$-向量空间$S_k(N, \chi)$和$S_\lambda^M(N, \chi)$有Hecke算子$T_n$作用在之上。这里$n \ge 1, (n, N) = 1$。且$T_1$为恒等算子。这些算子互相交换，并且对于Peterson内积是正规算子。如果$\chi$是平凡的，这些算子还是自伴算子。Peterson内积定义如下：对于$S_k(N, \chi)$的情形

$$\braket{f, g} = \iint_{\Gamma_0(N)/\mathcal H}f(z)\overline{g(z)}y^k\frac{dxdy}{y^2}$$

对于$S_\lambda^M(N, \chi)$的情形

$$\braket{f, g} = \iint_{\Gamma_0(N)/\mathcal H}f(z)\overline{g(z)}\frac{dxdy}{y^2}$$

对于$f \in S_k(N, \chi)$或$f \in S_\lambda^M(N, \chi)$，$f \not\equiv 0$，若

$$T_n f = \lambda_n\cdot f ~ \forall n \ge 1, (n, N) = 1$$

则称$f$为Hecke本征形式。可以证明，若$f$是Hecke本征形式，则$a_1 \ne 0$。假设$a_1 = 1$（这称为正规本征形式），则有

$$\lambda_n = a_n ~ \forall n \ge 1, (n, N) = 1$$

???+ note "注"
    对于$S_k(N, \chi)$的情形，Hecke算子满足以下恒等式： 
    
    $$\begin{aligned} T_{mn} = T_m \circ T_n ~ \forall (m, n) = 1, (m, N) = (n, N) = 1 \\ T_{p^{r+1}} = T_p \circ T_{p^r} - \chi(p)\cdot p^{k-1}T_{p^{r-1}} ~ \forall (p, N) = 1, r > 1 \end{aligned}$$
    
    $S_\lambda^M(N, \chi)$的情形类似。所以，若
    
    $$f = \sum_{n = 1}^\infty a_nq^n \in S_k(N, \chi)$$
    
    为正规本征形式时，我们有：
    
    $$\begin{aligned} a_{mn} = a_m \cdot a_n ~ \forall (m, n) = 1, (m, N) = (n, N) = 1 \\ a_{p^{r+1}} = a_p \cdot a_{p^r} - \chi(p)\cdot p^{k-1}a_{p^{r-1}} ~ \forall (p, N) = 1, r > 1 \end{aligned}$$
    
    $S_\lambda^M(N, \chi)$的情形类似。

???+ note "注"
    关于之前讨论的三次同余方程，设$P$为$\mathbb
    Q$上的三次不可约多项式，假设其判别式$\Delta$不是平方数（即其Galios群同构于$S_3$），则
    
    - 若$P$有一实根和两个非实根，那么$P$对于的Hecke本征形式是在$S_1(N, \chi)$。
    - 若$P$的所有根的都是实根，则$P$对应的Hecke本征形式是在$S_{1/4}^M(N, \chi)$。且有$a_n = a_{-n}$。

    其中$N$为Artin导子。$a_1 = 1$，且当$p \nmid N$时，有$N_p = 1+ a_p$。

???+ note "注"
    严格来说，Dirichlet特征在非交换情形的推广是Hecke本征形式。

## Ramanujan猜想

\proposition
    设$f \in S_k(N, \chi)$或$S_\lambda^M(N, \chi)$为Hecke本征形式。$a_1 = 1$，对于所有$p \nmid N$，设$\alpha_p, \beta_p$为二次多项式
    
    $$x^2 - a_px +\chi(p)\cdot p^{k-1}$$
    
    （Maass形式的情形则为$x^2 - a_px + \chi(p)$）的根，则我们有$|\alpha_p| = |\beta_p| = p^{\frac{k - 1}{2}}$，这就意味着
    
    $$|a_p| \le 2p^{\frac{k - 1}{2}}$$
    
    （Maass形式的情形则有$|a_p| \le 2$）。

![比利时邮局为纪念Deligne证明了Ramanujan猜想而发行的邮票](../assets/images/20210215183613239.gif){loading=lazy}
/// caption
比利时邮局为纪念Deligne证明了Ramanujan猜想而发行的邮票
///

结果：全纯形式$S_k(N, \chi)$为被Deligne于1974年所证的Weil猜想的推论。
Maass形式$S_\lambda^M(N, \chi)$时，目前最好的结果为Kim-Sarnak给出的

$$p^{-\frac{7}{64}} \le |\alpha_p|, |\beta_p| \le p^{\frac{7}{64}}$$

## 椭圆曲线

<aside>椭圆曲线上的点构成交换群，且恒等元为无穷远点。</aside>

$$y^2 = P(x)$$

其中$P(x)$为有理系数的三次多项式，且没有重根。

考虑方程$y^2 = x^3 - x^2 + \frac{1}{4}$，做变换$y \to y + \frac{1}{2}$，就有

$$y^2 + y = x^3 - x^2$$

该方程对于素数$p \ne 11$没有重根。定义

$$N_p = \#\{(x, y) \in \mathbb F_p^2 \mid y^2 + y \equiv x^3 - x^2 \mod p\}$$

我们有Hasse不等式

$$|N_p - p| < 2\sqrt p, ~ (p \ne 11)$$

我们要问同样的
!!! question "问题"
    随着$p$的变动，$N_p$的规律是什么？

对于这个问题，有如下（Eichler，1950's）

\theorem
    $$N_p = p - a_p$$

    这里

    $$q\prod_{n=1}^\infty[(1-q^n)(1-q^{11n})]^2 = \sum_{n = 1}^\infty a_nq^n, ~ a_1 = 1$$

    是属于$S_2(11, \mathcal 1)$的Hecke本征形式。

### 谷山-志村(Taniyama-Shimura)猜想

\proposition
    对于所有定义在$\mathbb Q$上的椭圆曲线，均有类似的情形成立。

该猜想在Semi-stable的情形由Wiles，Taylor-Wiles等人所证明。
Ribet再此之前已经证明从谷山-志村猜想可以推出著名的费马大定理。

???+ note "注"
    Wiles的证明也是需要用到Langlands(1980)的结果，Base Change for $\mathrm{GL}(2)$。
    更多信息可参考维基百科关于函子性猜想，Endoscopy以及基本引理（吴宝珠，2008）的相关内容。

## 总结

横向为Weil的所谓罗塞塔石碑，纵向为Langlands对应。

| 数论 | 有限域平面上的曲线 | 黎曼曲面 | 量子物理 |
|--|--|--|--|
| Langlands关系 |  Langlands关系 | 几何Langlands关系 | 电磁对偶性、镜像对称 |
| Galios群的表示 |  Galios群的表示 | $X$的基本群 | $X$的基本群 |
| Galios群在$L_G$中的表示 |  Galios群在$L_G$中的表示 | 基本群在$L_G$中的表示 | $M(L_G, X)$上的零膜 |
| 自守函数 |  自守函数或自守层 | 自守层 | $M(G, X)$上的$X$膜 |

## 参考文献

- E. Frenkel: Love and Math, The Heart of Hidden Reality, Basic Books, 2013.
- http://www.sunsite.ubc.ca/DigitalMathArchive/Langlands/functoriality.html
- https://www.bilibili.com/video/BV1sv411r7tc
- https://en.wikipedia.org/wiki/Langlands_program
