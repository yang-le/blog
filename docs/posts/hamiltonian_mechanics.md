---
date:
  created: 2024-11-12
categories:
    - 物理
---

# 哈密顿力学

## 勒让德变换和正则方程

作为坐标和速度的函数的拉格朗日函数，其全微分等于

$$d L = \frac{\partial L}{\partial q_i}dq_i + \frac{\partial L}{\partial\dot q_i}d\dot q_i$$

<!-- more -->

因为按定义导数$\partial L / \partial\dot q_i$是广义动量，又根据拉格朗日方程有$\partial L / \partial q_i = \dot p_i$，所以上面这个表达式可以写成

$$dL = \dot p_idq_i + p_id\dot q_i$$

现在将上式的第二项写成

$$p_id\dot q_i = d(p_i\dot q_i) - \dot q_idp_i$$

将全微分$d(p_i\dot q_i)$移到等式左边并改变符号，可得

$$d(p_i\dot q_i - L) = \dot q_idp_i - \dot p_idq_i$$

注意到上式右边不含$d\dot q_i$，可知微分的变量是用广义坐标和广义动量表示的系统的能量，称为系统的哈密顿函数

$$H(p, q, t) = p_i\dot q_i - L$$

由其中独立变量为坐标和动量的微分等式

$$dH = \dot q_idp_i - \dot p_idq_i$$

可以得出方程

$$\dot q_i = \frac{\partial H}{\partial p_i}, ~ \dot p_i = -\frac{\partial H}{\partial q_i}$$

这些就是用变量$p$和$q$表示的所要求的运动方程，称为哈密顿方程。它们是关于$2s$个未知函数$p_i(t)$和$q_i(t)$的$2s$个一阶微分方程组，代替拉格朗日方法的$s$个二阶方程。由于这些方程的形式简单并且对称，也称之为正则方程。

## 能量守恒

哈密顿函数对时间的全导数为

$$\frac{dH}{dt} = \frac{\partial H}{\partial t} + \frac{\partial H}{\partial q_i}\dot q_i + \frac{\partial H}{\partial p_i}\dot p_i$$

将正则方程代入，上式后两项抵消，因此

$$\frac{dH}{dt} = \frac{\partial H}{\partial t}$$

特别地，如果哈密顿函数不显含时间，则$dH / dt = 0$，即得到能量守恒定律。

## 泊松括号

设$f(p, q, t)$是坐标、动量和时间的某个函数。它对时间的全导数为

$$\frac{df}{dt} = \frac{\partial f}{\partial t} + \frac{\partial f}{\partial q_i}\dot q_i + \frac{\partial f}{\partial p_i}\dot p_i$$

代入由哈密顿方程给出的$\dot q, \dot p$的表达式，得

$$\frac{df}{dt} = \frac{\partial f}{\partial t} + \{H, f\}$$

其中引入了记号

$$\{H, f\} = \frac{\partial H}{\partial p_i}\frac{\partial f}{\partial q_i} - \frac{\partial H}{\partial q_i}\frac{\partial f}{\partial p_i}$$

该记号称为量$H$和$f$的泊松括号。
我们知道，如果动力学变量的某个函数当系统运动时保持不变，则称之为运动积分。由前述可知，$f$是运动积分($df / dt = 0$)的条件可以写为

$$\frac{\partial f}{\partial t} + \{H, f\} = 0$$

如果运动积分不显含时间，则

$$\{H, f\} = 0$$

即运动积分$f$和哈密顿函数的泊松括号必等于零。
对于任意一对变量$f, g$，泊松括号可以类似地定义为

$$\{f, g\} = \frac{\partial f}{\partial p_i}\frac{\partial g}{\partial q_i} - \frac{\partial f}{\partial q_i}\frac{\partial g}{\partial p_i}$$

### 性质

由定义容易推出泊松括号的如下性质

$$\begin{array}{ll}
\{f, g\} &= -\{g, f\} \\
\{f, c\} &= 0 \\
\{f_1 + f_2, g\} &= \{f_1, g\} + \{f_2, g\} \\
\{f_1f_2, g\} &= f_1\{f_2, g\} + f_2\{f_1, g\} \\
\displaystyle\frac{\partial}{\partial t}\{f, g\} &= \{\displaystyle\frac{\partial f}{\partial t}, g\} + \{f, \displaystyle\frac{\partial g}{\partial t}\}\\
\end{array}$$

如果函数$f$或$g$之一是广义坐标或者广义动量，则泊松括号简化为偏导数:

$$\begin{array}{ll}
\{f, q_i\} &= \displaystyle\frac{\partial f}{\partial p_i} \\
\{f, p_i\} &= -\displaystyle\frac{\partial f}{\partial q_i} \\
\end{array}$$

作为上式的特殊情况，有

$$\begin{array}{ll}
\{q_i, q_j\} &= 0 \\
\{p_i, p_j\} &= 0 \\
\{p_i, q_j\} &= \delta_{ij}
\end{array}$$

在三个函数组成的泊松括号之间，存在下面的关系式

$$\{f, \{g, h\}\} + \{g, \{h, f\}\} + \{h, \{f, g\}\} = 0$$

我们称之为雅可比恒等式。

### 泊松定理

泊松括号的一个重要性质在于，如果$f$和$g$是两个运动积分，则它们构成的泊松括号也是运动积分

$$\{f, g\} = const$$

这就是泊松定理。

## 正则变换

下面研究变换

$$Q = (p, q, t), ~ P = (p, q,t)$$

需要满足什么条件，才能保持运动方程的如下形式

$$\dot Q = \frac{\partial H'}{\partial P}, ~ \dot P = -\frac{\partial H'}{\partial Q}$$

其中$H'(P, Q)$是某个哈密顿函数。在这些变换中有一类特别重要的变换，称之为正则变换。
根据拉格朗日量和哈密顿量的关系，最小作用量原理可以表述为

$$\delta\int(p dq - Hdt) = 0$$

如果新变量$P$和$Q$也满足哈密顿方程，则下面的最小作用量原理也必须成立:

$$\delta\int(P dQ - H'dt) = 0$$

如果上述两式的被积函数仅相差关于坐标、动量和时间的某个函数$F$的全微分，则两种形式当然是等价的。因此，我们将取关系

$$pdq - Hdt = PdQ - H'dt + dF$$

满足这个条件的变换称为正则变换。每一个正则变换均由称之为变换的母函数的这个特定函数$F$来表征。
将所得关系式写成

$$dF = pdq - PdQ + (H' - H)dt$$

可以看出

$$p = \frac{\partial F}{\partial q}, ~ P = -\frac{\partial F}{\partial Q}, ~ H' = H + \frac{\partial F}{\partial t}$$

这里假设$F = F(q, Q, t)$。当函数$F$已知时，上述公式给出了老变量$(p, q)$和新变量$(P, Q)$的关系，同时还给出新的哈密顿函数。

以上是用变量$(q, Q)$表示母函数，当然也可以用其他变量表示母函数，只要其中一个来自老变量，另一个来自新变量即可。如此便共有四类母函数，前面介绍的是第一类。为了推导其他三类变换公式，我们需要做适当的勒让德变换，例如为了把$P, Q$交换位置，可以做如下变换

$$d(F + PQ) = pdq + QdP + (H' - H)dt$$

如此新的母函数就是用变量$(q, P)$表示的了，记这个新的母函数为$F_1(q, P, t) \equiv F + PQ$，我们有

$$p = \frac{\partial F_1}{\partial q}, ~ Q = \frac{\partial F_1}{\partial P}, ~ H' = H + \frac{\partial F_1}{\partial t}$$

类似地，可以得到其他两个母函数，$F_2(p, Q, t) \equiv F - pq$，且有

$$q = -\frac{\partial F_2}{\partial p}, ~ P = -\frac{\partial F_2}{\partial Q}, ~ H' = H + \frac{\partial F_2}{\partial t}$$

以及$F_3(p, P, t) \equiv F + PQ - pq$，且有

$$q = -\frac{\partial F_3}{\partial p}, ~ Q = \frac{\partial F_3}{\partial P}, ~ H' = H + \frac{\partial F_3}{\partial t}$$

下面举几个例子

### 点变换

$$F_1 = Pf(q, t)$$

其中$Q = f(q, t)$是任意函数，这其实就是点变换。这说明点变换是正则变换的特殊情况。特别当$f(q, t) = q$时，这就是恒等变换。

### 共轭变量

$$F= qQ$$

将导出$p = Q, ~ P = -q$，这相当于将坐标和动量互换。
正则变换的广泛性，在很大程度上使哈密顿方法中的广义坐标和广义动量的概念丧失其原始含义。由于变换将变量$P, Q$中的每一个都同坐标$q$和动量$p$联系在一起，所以变量$Q$不再是纯粹的空间坐标。$Q$和$P$两者之间的区别本质上仅在于名称的不同，因此这两个变量在哈密顿方法中经常被简称为正则共轭变量。联系这些共轭变量的条件仍然可以用先前得到的一个结果表示

$$\{Q_i, Q_j\} = 0, ~ \{P_i, P_j\} = 0, ~ \{P_i, Q_j\} = \delta_{ij}$$

### 运动本身

在运动中变量$p, q$的变化本身也可以看作是一系列的正则变换。这个结论的含义如下。设$q_t, p_t$是正则变量在$t$时刻的值，而$q_{t + \tau}, p_{t + \tau}$是正则变量在另一个时刻$t + \tau$的值。后者是前者(以及作为参量的时间间隔$\tau$)的某种函数：

$$q_{t + \tau} = q(q_t, p_t, t; \tau), ~ p_{t + \tau} = p(q_t, p_t, t; \tau)$$

因为$dS = pdq - Hdt$，考虑整个运动过程中从$q_t$到$q_{t + \tau}$的这一段就有

$$dS = p_{t + \tau}dq_{t + \tau} - H_{t + \tau}dt - (p_tdq_t - H_tdt)$$

稍作变形

$$p_tdq_t - H_tdt = p_{t + \tau}dq_{t + \tau} - H_{t + \tau}dt - dS$$

如果将这些公式看作从变量$q_t, p_t$到变量$q_{t + \tau}, p_{t + \tau}$的变换，则这是正则变换，而$-S$就是变换的母函数。

## 刘维尔定理

为了给出力学现象的几何解释，经常用到相空间的概念，相空间是以所涉及的力学系统的$s$个广义坐标和$s$个广义动量为坐标轴的$2s$维空间。相空间的每个点对应于系统的一个确定状态。当系统运动时，表示系统状态的相点在相空间中画出的曲线称为相轨道。刘维尔定理断言，相空间的体积元在正则变换下是守恒的，也就是说

$$\int dqdp = \int dQdP$$

证明如下，根据多重积分的换元公式

$$\int dqdp = \int JdQdP$$

其中$J$是变换的雅可比行列式，而根据雅可比行列式的性质有

$$J \equiv \frac{\partial (Q, P)}{\partial (q, p)} = \frac{\partial (Q, P)}{\partial (q, P)} / \frac{\partial (q, p)}{\partial (q, P)} = \frac{\partial Q}{\partial q} / \frac{\partial p}{\partial P}$$

而根据第1型母函数的性质，有

$$\frac{\partial Q}{\partial q} = \frac{\partial p}{\partial P} = \frac{\partial^2F_1}{\partial q\partial P}$$

因此，$J = 1$，定理得证。
由于运动本身也可以看作是正则变换，因此在运动过程中，相点的密度也是守恒的。

## 哈密顿-雅可比方程

假若可以找到一个第1型母函数$S= F_1$使得新哈密顿量$H'$恒等于0.则该生成函数$S(q, P, t)$为哈密顿主函数。由于新哈密顿量$H'$所有的偏导数都等于0，哈密顿方程也变得非常简单：

$$\dot P = \dot Q = 0$$

这时，新的正则坐标都成为运动积分$a, b$:

$$P = a, Q = b$$

由于$p = \frac{\partial S}{\partial q}$，代入旧哈密顿量，则可得:

$$H(q, \frac{\partial S}{\partial q}, t) + \frac{\partial S}{\partial t} = 0$$

这是哈密顿主函数$S$应该满足的一阶偏微分方程， 称为哈密顿-雅可比方程。

### 哈密顿特征函数

前面提到，如果哈密顿量不显含时间，则有能量守恒，即$H(q, p) = E$，则

$$\frac{\partial S}{\partial t} = H' - H = -E$$

哈密顿主函数就可以分离成两部分:

$$S = W(q, a) - Et$$

其中，不含时间的函数$W(q, a)$称为哈密顿特征函数。此时，哈密顿-雅可比方程可进一步简化为

$$H(q, \frac{\partial W}{\partial q}) = E$$

### 分离变量法

给定一个$n$元函数$F(x^i)$的偏微分方程，有时候，为了将问题的偏微分方程改为一组常微分方程，可以猜想一个解答；解答的形式为

$$F = \sum f_i(x_i)$$

或者

$$F = \prod f_i(x_i)$$

时常，对于每一个自变量$x_i$，都会伴随着一个分离常数。如果这个方法成功，则称此偏微分方程为可分偏微分方程。
假设某个坐标，例如用$q_1$表示，与相应的导数$\partial S / \partial q_1$，在哈密顿-雅可比方程中仅以某种组合$\varphi(q_1, \partial S / \partial q_1)$的形式出现，该组合中不包含其他坐标，时间及其导数，即方程的形式为

$$H\{q_i, \frac{\partial S}{\partial q_i}, \varphi(q_1, \frac{\partial S}{\partial q_1}), t\} + \frac{\partial S}{\partial t} = 0$$

其中$q_i$表示出了$q_1$之外的所有坐标。现在，我们猜想哈密顿-雅可比方程存在如下形式的解：

$$S = S'(q_i, t) + S_1(q_1)$$

将此代入原方程得

$$H\{q_i, \frac{\partial S'}{\partial q_i}, \varphi(q_1, \frac{\partial S_1}{\partial q_1}), t\} + \frac{\partial S'}{\partial t} = 0$$

如果我们猜想的解是正确的，那么上式应该为恒等式（而不管坐标$q_1$如何变化）。由于$q_1$只影响$\varphi$的值，这就要求$\varphi$是常数。于是上述方程就分离为两个方程

$$\begin{array}{ll}
\varphi(q_1, \frac{\partial S_1}{\partial q_1}) &= \alpha_1 \\
H(q_i, \frac{\partial S'}{\partial q_i}, \alpha_1, t) + \frac{\partial S'}{\partial t} &= 0
\end{array}$$

其中$\alpha_1$是任意常数。上面第一个是常微分方程，由此方程通过简单的积分可以求出函数$S_1(q_1)$。剩下的一个是偏微分方程，但是它的独立变量的数目减少了。
如果用这样的方法可以相继地分离所有$s$个坐标和时间，则求哈密顿-雅可比方程就约化为求积分了。对于保守系统实际上只需分离方程中的$s$个变量(坐标)，在完全分离的情况下，方程的解可以写为

$$S = \sum S_i(q_i;\alpha_1, \cdots, \alpha_s) - E(\alpha_1, \cdots, \alpha_s)t$$

其中每个$S_i$都只是一个坐标的函数，而能量$E$是任意常数$\alpha_1, \cdots, \alpha_s$的函数，可以通过将$S_0 = \sum S_i$代入原方程求得。
一个特殊情况是循环变量的分离。循环变量是指不显含于哈密顿函数的变量，因此也不显含于哈密顿-雅可比方程。考虑此时方程的形式，就可以看出$\varphi(q_1, \partial S / \partial q_1)$实际上就是$\partial S / \partial q_1$。因此$S_1 = \alpha_1q_1$，即

$$S = S'(q_i, t) + \alpha_1q_1$$

常数$\alpha_1$正是相应于循环坐标的常值动量$p_1 = \partial S / \partial q_1$。对于一个保守系统而言，$-Et$这一项中出现的时间$t$相应于对“循环变量”$t$的分离。
作为例子，我们现在使用几种正交坐标系进行分离变量的计算。

#### 球坐标

球坐标的坐标变换关系如下

$$\begin{array}{l}
	x = r\sin\theta\sin\varphi \\
	y = r\sin\theta\cos\varphi \\
	z = r\cos\theta
\end{array}$$

它们的导数(速度)之间的关系为

$$\begin{array}{l}
	\dot x = \sin\theta\sin\varphi\dot r + r\cos\theta\sin\varphi\dot\theta + r\sin\theta\cos\varphi\dot\varphi \\
	\dot y = \sin\theta\cos\varphi\dot r + r\cos\theta\cos\varphi\dot\theta - r\sin\theta\sin\varphi\dot\varphi \\
	\dot z = \cos\theta\dot r - r\sin\theta\dot\theta
\end{array}$$

当$\theta$和$\varphi$为常数时，系统的整体速度就是径向速度，同样的道理

$$\begin{array}{ll}
\vec v_r &= (\sin\theta\sin\varphi, \sin\theta\cos\varphi, \cos\theta)\dot r \\
\vec v_\theta &= (\cos\theta\sin\varphi, \cos\theta\cos\varphi, -\sin\theta)r\dot\theta \\
\vec v_\varphi &= (\cos\varphi, -\sin\varphi, 0)r\sin\theta\dot\varphi
\end{array}$$

注意到上面括号里都是单位矢量，其方向就是对应速度的方向，把它们分别记作$\hat{\vec r}, \hat{\vec\theta}, \hat{\vec\varphi}$，就有

$$\begin{array}{ll}
\vec v_r &= \dot r\hat{\vec r} \\
\vec v_\theta &= r\dot\theta\hat{\vec\theta} \\
\vec v_\varphi &= r\sin\theta\dot\varphi\hat{\vec\varphi}
\end{array}$$

现在我们就可以写出系统的拉格朗日函数

$$L = \frac{1}{2}m(v_r^2 + v_\theta^2 + v_\varphi^2) = \frac{1}{2}m(\dot r^2 + r^2\dot\theta^2 + r^2\sin^2\theta\dot\varphi^2)$$

根据广义动量的定义，有

$$\begin{array}{ll}
p_r := \frac{\partial L}{\partial\dot r} = m\dot r \\
p_\theta := \frac{\partial L}{\partial\dot\theta} = r^2m\dot\theta \\
p_\varphi := \frac{\partial L}{\partial\dot\varphi} = r^2\sin^2\theta m\dot\varphi
\end{array}$$

所以拉格朗日函数也可以用广义动量表示为

$$L = \frac{1}{2m}(p_r^2 + \frac{p_\theta^2}{r^2} + \frac{p_\varphi^2}{r^2\sin^2\theta})$$

接下来就是把上式代入哈密顿-雅可比方程，得到

$$\frac{1}{2m}[(\frac{\partial S}{\partial r})^2 + \frac{1}{r^2}(\frac{\partial S}{\partial \theta})^2 + \frac{1}{r^2\sin^2\theta}(\frac{\partial S}{\partial \varphi})^2] + U(r, \theta, \varphi) + \frac{\partial S}{\partial t} =0$$

假如位势函数$U$的形式可以进一步设定为

$$U(r, \theta, \varphi) = U_r + \frac{U_\theta}{r^2} + \frac{U_\varphi}{r^2\sin^2\theta}$$

则方程就是完全可分的。将方程的解$S = S_r + S_\theta + S_\varphi -Et$代入，得

$$[(\frac{dS_r}{dr})^2 + 2mU_r] + \frac{1}{r^2}[(\frac{dS_\theta}{d\theta})^2 + 2mU_\theta] + \frac{1}{r^2\sin^2\theta}[(\frac{dS_\varphi}{d\varphi})^2 + 2mU_\varphi] = 2mE$$

现在，由于$\varphi$只在上式第三个方括号内出现，因此这部分就应该是一个运动常数，记作

$$(\frac{dS_\varphi}{d\varphi})^2 + 2mU_\varphi = \Gamma_\varphi$$

简化后的方程就变为

$$[(\frac{dS_r}{dr})^2 + 2mU_r] + \frac{1}{r^2}[(\frac{dS_\theta}{d\theta})^2 + 2mU_\theta + \frac{\Gamma_\varphi}{sin^2\theta}] = 2mE$$

类似地，可以得到

$$(\frac{dS_\theta}{d\theta})^2 + 2mU_\theta + \frac{\Gamma_\varphi}{sin^2\theta} = \Gamma_\theta$$

最后剩下一个关于径向距离函数$S_r$的常微分方程

$$(\frac{dS_r}{dr})^2 + 2mU_r + \frac{\Gamma_\theta}{r^2} = 2mE$$

积分后得

$$S = \int\sqrt{2m(E-U_r) - \frac{\Gamma_\theta}{r^2}} ~ dr + \int\sqrt{\Gamma_\theta - 2mU_\theta - \frac{\Gamma_\varphi}{\sin^2\theta}} ~ d\theta + \int\sqrt{\Gamma_\varphi - 2mU_\varphi} ~ d\varphi - Et$$

$U_\varphi$未必有物理意义，可取为0。如果是这样，那么$\varphi$就不显含于方程，是循环坐标，那么很容易就算出最后一项积分为$p_\varphi\varphi$。

#### 抛物线坐标

物理上的抛物线坐标定义如下

$$\begin{array}{l}
	x = \sqrt{\eta\xi}\cos\varphi \\
	y = \sqrt{\eta\xi}\sin\varphi  \\
	z = \frac{1}{2}(\xi - \eta)
\end{array}$$

经过类似的运算，可以得到哈密顿-雅可比方程为

$$\frac{2}{m(\xi + \eta)}[\xi(\frac{\partial S}{\partial\xi})^2 + \eta(\frac{\partial S}{\partial\eta})^2] + \frac{1}{2m\xi\eta}(\frac{\partial S}{\partial\varphi})^2 + U(\xi, \eta, \varphi) + \frac{\partial S}{\partial t} = 0$$

假如位势函数$U$的形式可以进一步设定为

$$U(\xi, \eta, \varphi) = \frac{U_\xi + U_\eta}{\xi + \eta} + \frac{U_\varphi}{\xi\eta}$$

则方程就是完全可分的。将方程的解$S = S_\xi + S_\eta+ S_\varphi -Et$代入，得

$$2[\xi(\frac{dS_\xi}{d\xi})^2 + \eta(\frac{dS_\eta}{d\eta})^2] + \frac{1}{2}(\frac{1}{\xi} + \frac{1}{\eta})(\frac{dS_\varphi}{d\varphi})^2 + m(U_\xi + U_\eta) + m(\frac{1}{\xi} + \frac{1}{\eta})U_\varphi - m(\xi + \eta)E = 0$$

记

$$(\frac{dS_\varphi}{d\varphi})^2 + 2mU_\varphi = \Gamma_\varphi$$

余下方程为

$$2\xi(\frac{dS_\xi}{d\xi})^2 + mU_\xi - mE\xi + \frac{\Gamma_\varphi}{2\xi} + 2\eta(\frac{dS_\eta}{d\eta})^2 + mU_\eta - mE\eta + \frac{\Gamma_\varphi}{2\eta} = 0$$

如果我们记

$$2\xi(\frac{dS_\xi}{d\xi})^2 + mU_\xi - mE\xi + \frac{\Gamma_\varphi}{2\xi} = \Gamma$$

那么就有

$$2\eta(\frac{dS_\eta}{d\eta})^2 + mU_\eta - mE\eta + \frac{\Gamma_\varphi}{2\eta} = -\Gamma$$

逐个积分得

$$S = \int\sqrt{\frac{mE}{2} + \frac{\Gamma}{2\xi} -  \frac{mU_\xi}{2\xi} - \frac{\Gamma_\varphi}{4\xi^2}} ~ d\xi + \int\sqrt{\frac{mE}{2} - \frac{\Gamma}{2\eta} - \frac{mU_\eta}{2\eta} - \frac{\Gamma_\varphi}{4\eta^2}} ~ d\eta + \int\sqrt{\Gamma_\varphi - 2mU_\varphi} ~ d\varphi - Et$$

#### 椭圆坐标(长球面坐标)

物理上的定义是

$$\begin{array}{l}
	x = \sigma\sqrt{(\xi^2 - 1)(1 -\eta^2)}\cos\varphi \\
	y = \sigma\sqrt{(\xi^2 - 1)(1 -\eta^2)}\sin\varphi \\
	z = \sigma\xi\eta
\end{array}$$

其中常数$\sigma$是变换参数。
哈密顿-雅可比方程为

$$\frac{1}{2m\sigma^2(\xi^2 - \eta^2)}[(\xi^2 - 1)(\frac{\partial S}{\partial\xi})^2 + (1 - \eta^2)(\frac{\partial S}{\partial\eta})^2 + (\frac{1}{\xi^2 - 1} + \frac{1}{1 - \eta^2})(\frac{\partial S}{\partial\varphi})^2 ] + U(\xi, \eta, \varphi) + \frac{\partial S}{\partial t} = 0$$

假如有

$$U = \frac{U_\xi + U_\eta}{\xi^2 - \eta^2} + \frac{U_\varphi}{(\xi^2 - 1)(1 -\eta^2)}$$

则方程就是完全可分的。将方程的解$S = S_\xi + S_\eta+ S_\varphi -Et$代入，得

$$(\xi^2 - 1)(\frac{dS_\xi}{d\xi})^2 + 2m\sigma^2U_\xi - 2m\sigma^2E(\xi^2 - 1) + (1 - \eta^2)(\frac{dS_\eta}{d\eta})^2 + 2m\sigma^2U_\eta - 2m\sigma^2E(1 - \eta^2)+ (\frac{1}{\xi^2 - 1} + \frac{1}{1 - \eta^2})[(\frac{dS_\varphi}{d\varphi})^2 + 2m\sigma^2U_\varphi] = 0$$

记

$$(\frac{dS_\varphi}{d\varphi})^2 + 2m\sigma^2U_\varphi = \Gamma_\varphi$$

余下方程为

$$(\xi^2 - 1)(\frac{dS_\xi}{d\xi})^2 + 2m\sigma^2U_\xi - 2m\sigma^2E(\xi^2 - 1) + \frac{\Gamma_\varphi}{\xi^2 - 1} + (1 - \eta^2)(\frac{dS_\eta}{d\eta})^2 + 2m\sigma^2U_\eta - 2m\sigma^2E(1 - \eta^2) + \frac{\Gamma_\varphi}{1 - \eta^2} = 0$$

如果我们记

$$(\xi^2 - 1)(\frac{dS_\xi}{d\xi})^2 + 2m\sigma^2U_\xi - 2m\sigma^2E(\xi^2 - 1) + \frac{\Gamma_\varphi}{\xi^2 - 1} = \Gamma$$

那么就有

$$(1 - \eta^2)(\frac{dS_\eta}{d\eta})^2 + 2m\sigma^2U_\eta - 2m\sigma^2E(1 - \eta^2) + \frac{\Gamma_\varphi}{1 - \eta^2} = -\Gamma$$

逐个积分得

$$S = \int\sqrt{2m\sigma^2E + \frac{\Gamma - U_\xi}{\xi^2 -1} -\frac{\Gamma_\varphi}{(\xi^2 - 1)^2}} ~ d\xi + \int\sqrt{2m\sigma^2E - \frac{\Gamma + U_\eta}{1 - \eta^2} -\frac{\Gamma_\varphi}{(1 - \eta^2)^2}} ~ d\eta + \int\sqrt{\Gamma_\varphi - 2m\sigma^2U_\varphi} ~ d\varphi - Et$$