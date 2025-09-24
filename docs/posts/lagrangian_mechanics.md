---
date:
  created: 2024-11-12
categories:
    - 物理
---

# 拉格朗日力学

## 广义坐标和位形空间

$N$维系统有$N$个独立的广义坐标，每组广义坐标$(q^1, q^2, \cdots, q^N)$确定系统的一个位形(configuration)，因此广义坐标又称位形变量。所有可能的位形的集合称为系统的位形空间，是个$N$维流形。系统的演化($q^i$随时间$t$的变化)对应于位形空间中一条以$t$为参数的曲线$\eta(t)$，其参数式为$q^i = q^i(t)$。曲线的切矢代表演化的速度，其坐标分量$\dot q^i(t) \equiv \frac{dq^i(t)}{dt}$就是通常所称的广义速度。设$Q_0 \equiv \eta(t_0), Q_1 \equiv \eta(t_1)$且$t_1 > t_0$，则$\eta(t)$介于$Q_0$和$Q_1$之间的一段反映系统从初始位形$Q_0$到终了位形$Q_1$的演化。从$Q_0$到$Q_1$的每一曲线称为一条路径，由$N$个排了序的一元函数$q^i(t)$决定，满足

$$(q^1(t_0), \cdots, q^N(t_0)) = Q_0, ~ (q^1(t_1), \cdots, q^N(t_1)) = Q_1$$

<!-- more -->

所有路径中只有$\eta(t)$才是演化曲线，才代表由动力学规律决定的演化过程。一个最简单的例子是牛顿力学中由一个自由质点构成的系统，其位形空间就是$\R^3$，任给$Q_0$和$Q_1$(满足$t_0 < t_1$)后，两点之间的直线便是演化曲线。

## 变分原理

系统的拉格朗日量$L$是广义坐标$q^i$和广义速度$\dot q^i$的函数，即$L = L(q^i, \dot q^i, t)$。对于每一条路径$\{q^1(t), \cdots, q^N(t)\}$，$L$又通过宗量$q^i, \dot q^i, t$成为$t$的一元函数，其积分$S$称为该路径的作用量

$$S := \int_{t_0}^{t_1}L(q^i(t), \dot q^i(t), t)dt$$

演化曲线的不同之处由变分原理给出，它要求演化曲线的作用量取最小值。考虑任一(从$Q_0$到$Q_1$的)单参路径族$q^i = q^i(t, \lambda)$，参数$\lambda = 0$给出演化路径$\eta(t)$, $\lambda \ne 0$则给出其他路径。族中曲线的作用量$S$由于$q^i, \dot q^i$依赖于$\lambda$而成为$\lambda$的函数

$$S(\lambda) = \int_{t_0}^{t_1}L(q^i(t, \lambda), \dot q^i(t, \lambda), t)dt$$

于是$S$在单参族内的求最小值问题便归结为一元函数$S(\lambda)$的求导问题

$$\frac{dS(\lambda)}{d\lambda}|_{\lambda = 0} = \int_{t_0}^{t_1}\frac{dL}{d\lambda}|_{\lambda = 0}dt = \int_{t_0}^{t_1}(\frac{\partial L}{\partial q^i}\frac{\partial q^i}{\partial\lambda} + \frac{\partial L}{\partial\dot q^i}\frac{\partial\dot q^i}{\partial\lambda})|_{\lambda = 0}dt$$

令$\delta S \equiv \frac{dS(\lambda)}{d\lambda}|_{\lambda = 0}, ~ \delta q^i \equiv \frac{\partial q^i(t, \lambda)}{\partial \lambda}|_{\lambda = 0}, ~ \delta\dot q^i \equiv \frac{\partial\dot q^i(t, \lambda)}{\partial \lambda}|_{\lambda = 0}$，并把$\delta S, \delta q^i$和$\delta\dot q^i$分别称为$S, q^i$和$\dot q^i$在所选单参族内的变分，则上式对应如下变分关系

$$\delta S = \int_{t_0}^{t_1}(\frac{\partial L}{\partial q^i}\delta q^i + \frac{\partial L}{\partial\dot q^i}\delta\dot q^i)dt$$

其中$\frac{\partial L}{\partial q^i}$和$\frac{\partial L}{\partial\dot q^i}$是$\frac{\partial L}{\partial q^i}|_{\lambda = 0}$和$\frac{\partial L}{\partial\dot q^i}|_{\lambda = 0}$的简写。因为演化曲线$\eta(t)$有$\lambda = 0$，变分原理的含义就是所有单参路径族的$\delta S$都为零。下面导出$\delta S = 0$(对所有单参族)的等价条件。由于对$\lambda$求导和对$t$求导可交换顺序，被积函数的第二项可改写为

$$\frac{\partial L}{\partial\dot q^i}\delta\dot q^i = \frac{\partial L}{\partial\dot q^i}\frac{d}{dt}\delta q^i$$

由分部积分法得

$$\int_{t_0}^{t_1}\frac{\partial L}{\partial\dot q^i}\delta\dot q^idt = (\frac{\partial L}{\partial\dot q^i}\delta q^i)|_{t_0}^{t_1} - \int_{t_0}^{t_1}\delta q^i\frac{d}{dt}\frac{\partial L}{\partial\dot q^i}dt$$

因为所有曲线的起、止点都是$Q_0$和$Q_1$，所以有

$$\delta q^i|_{t_0} = \lim_{\lambda \to 0}\frac{1}{\lambda}[q^i(t_0, \lambda) - q^i(t_0, 0)] = \lim_{\lambda \to 0}\frac{1}{\lambda}[Q_0 - Q_0] = 0$$

和$\delta q^i|_{t_1} = 0$。因而上式右边第一项为零，带入$\delta S$的表达式，就有

$$\delta S = \int_{t_0}^{t_1}(\frac{\partial L}{\partial q^i} - \frac{d}{dt}\frac{\partial L}{\partial\dot q^i})\delta q^idt$$

于是$\eta(t)$为演化曲线的必要条件为

$$\frac{\partial L}{\partial q^i} - \frac{d}{dt}\frac{\partial L}{\partial\dot q^i} = 0$$

此方程称为欧拉-拉格朗日方程。

## 拉格朗日量的性质

力学中的欧拉-拉格朗日方程

$$\frac{\partial L}{\partial q} = \frac{d}{dt}\frac{\partial L}{\partial\dot q}$$

是牛顿第二运动定律$F = \frac{dp}{dt}$的推广。其中$\frac{\partial L}{\partial q}$称为广义力，$\frac{\partial L}{\partial\dot q}$称为广义动量。
拉格朗日量有如下重要的性质

- 拉格朗日量是可加的，整体的拉格朗日量是部分的拉格朗日量之和。
- 拉格朗日量乘以一个任意常数，不改变系统的运动方程，这归结为对物理度量单位选择的任意性。
- 拉格朗日量加上一个关于时间和坐标的任意函数对时间全导数，不改变系统的运动方程。

因为

$$L'(q, \dot q, t) = L(q, \dot q, t) + \frac{d}{dt}f(q, t) $$

两边积分就有

$$S' = S + f(q^{(2)}, t_2) - f(q^{(1)}, t_1)$$

即$S$和$S'$相差一个常数项，该常数项在应用变分条件时将消失。

## 自由质点的拉格朗日函数

在惯性参考系中，空间可以认为是均匀且各向同性的，时间也是均匀流逝的。此时拉格朗日函数将于坐标和时间无关，而只能是关于速度的函数。由于空间各向同性，拉格朗日函数也必须不依赖速度的方向，因此只能是速度大小的函数，也就是说$L$是关于$\vec v^2 = v^2$的函数。由于$L$与$\vec r$无关，因此$\frac{\partial L}{\partial \vec r} = 0$，所以拉格朗日方程可以写为

$$\frac{d}{dt}\frac{\partial L}{\partial\vec v} = 0$$

因此$\frac{\partial L}{\partial\vec v}$为一常数。而$\frac{\partial L}{\partial\vec v}$仅为速度的函数，故速度的大小和方向都不发生改变。这就是牛顿第一运动定律。

考虑惯性参考系$K$以无穷小速度$\vec\epsilon$相对另一惯性参考系$K'$运动，则有$\vec v' = \vec v + \vec\epsilon$。拉格朗日函数$L(v^2)$经过伽利略变换后得到$L'$，由于在所有惯性参考系中运动方程的形式都相同，因此$L'$和$L$只能相差某个关于时间和坐标的函数的全导数，于是

$$L' = L(v'^2) = L(v^2 + 2\vec v\cdot\vec\epsilon + \epsilon^2)$$

将此表达式展开为$\vec\epsilon$的幂级数，并忽略一阶以上的无穷小量，得

$$L(v'^2) = L(v^2) + 2\frac{\partial L}{\partial v^2}\vec v\cdot\vec\epsilon$$

只有当右边第二项与速度$\vec v$呈线性依赖关系时，它才能时时间的全导数。因此$\frac{\partial L}{\partial v^2}$不依赖于速度，即该情况下拉格朗日函数与速度平方成正比

$$L = \frac{m}{2}v^2$$

其中$m$为常数。
由上述分析可知，在参考系$K$以有限速度$\vec V$相对$K'$运动的情况下，拉格朗日函数也满足伽利略相对性原理。事实上，

$$L' = \frac{m}{2}v'^2 = \frac{m}{2}(\vec v + \vec V)^2 = \frac{m}{2}v^2 + 2\frac{m}{2}\vec v \cdot \vec V + \frac{m}{2}V^2$$

或者

$$L' = L + \frac{d}{dt}(2\frac{m}{2}\vec r\cdot\vec V + \frac{m}{2}V^2t)$$

第二项是时间的全导数，可以略去。

上面的$m$称为质点的质量，容易看出，质量不可能是负的。因为，根据变分原理，积分

$$S = \int_{t_1}^{t_2}\frac{mv^2}{2}dt$$

应该取最小值。假如质量是负的，那么作用量就可以取绝对值任意大的负值，而不可能有最小值。

## 质点系的拉格朗日函数

### 封闭质点系

考虑封闭质点系，其质点间有相互作用，但不受外部任何物体作用。为描述这种情形，我们需要在自由质点的拉格朗日函数中增加坐标的某一函数。（考虑到与牛顿力学的相容性）将这个函数记为$-U$，则有

$$L = \sum_a\frac{m_av_a^2}{2} - U(\vec r_1, \vec r_2, \cdots)$$

其中$\vec r_a$是第$a$个质点的矢径。这是封闭质点系拉格朗日函数的一般形式。
函数$U$称为质点系的势能，而

$$T = \sum_a\frac{m_av_a^2}{2}$$

称为质点系的动能。
知道拉格朗日函数后就可以建立运动方程

$$\frac{d}{dt}\frac{\partial L}{\partial \vec v_a} = \frac{\partial L}{\partial \vec r_a}$$

代入前面得到的拉格朗日函数得:

$$m_a\frac{d\vec v_a}{dt} = -\frac{\partial U}{\partial\vec r_a}$$

这种形式的运动方程称为牛顿方程。右端的矢量

$$\vec F_a = -\frac{\partial U}{\partial\vec r_a}$$

称为作用在第$a$个质点上的力。它与$U$一样，只依赖于所有质点的坐标，而不依赖于速度。因此，上述方程表明，质点的加速度矢量也只是坐标的函数。
势能可以增减任意常数而不改变运动方程，这是前面讲到过的拉格朗日函数的最后一个性质的特殊情况。

### 非封闭质点系

对于非封闭质点系$A$，假设它受到运动完全已知的质点系$B$的作用。这种情况下称$A$在(由$B$产生的)给定的外场中运动。因为$B$是完全已知的，我们可以将质点系$A + B$的拉格朗日函数$L$中广义坐标$q_B$用给定的关于时间的函数代替，由此得到质点系$A$的拉格朗日函数$L_A$。
假设质点系$A + B$是封闭的，则有

$$L = T_A(q_A, \dot q_A) + T_B(q_B, \dot q_B) - U(q_A, q_B)$$

其中前两项分别是系统$A$和$B$的动能，第三项是$A + B$的势能。将广义坐标$q_B$用已知的时间函数代替后，$T_B(q_B, \dot q_B)$是只依赖于时间的函数(因此也是某个时间函数的全导数)，可以从$L$中略去。于是

$$L_A = T_A(q_A, \dot q_A) - U(q_A, q_B(t)) $$

可见，在外场中的质点系的运动由通常形式的拉格朗日函数描述，仅有的差别就在于势能可能显含时间。例如，对于在外场中运动的单个质点，拉格朗日函数的一般形式为

$$L = \frac{mv^2}{2} - U(\vec r, t)$$

而运动方程写成

$$m\dot{\vec v} = -\frac{\partial U}{\partial \vec r}$$

如果一质点在一个场中的任意位置都受到相同的力$\vec F$，则称这样的外场是均匀的。显然在均匀外场中的势能可以写成

$$U = -\vec F\cdot\vec r$$

## 守恒定律

### 能量

由于时间具有均匀性，封闭系统的拉格朗日函数不显含时间。因此其对时间的全导数可以写成

$$\frac{dL}{dt} = \frac{\partial L}{\partial q}\dot q + \frac{\partial L}{\partial \dot q}\ddot q$$

(否则上式右端还应该加上$\partial L / \partial t$)。利用拉格朗日方程将$\partial L / \partial q$替换为$\frac{d}{dt}\frac{\partial L}{\partial \dot q}$，得

$$\frac{dL}{dt} = \frac{d}{dt}\frac{\partial L}{\partial \dot q}\dot q + \frac{\partial L}{\partial \dot q}\frac{d}{dt}\dot q = \frac{d}{dt}(\frac{\partial L}{\partial \dot q}\dot q)$$

或者

$$\frac{d}{dt}(\dot q\frac{\partial L}{\partial \dot q} - L) = 0$$

由此可知

$$E = \dot q\frac{\partial L}{\partial \dot q} - L$$

在封闭系统运动中保持不变，称为系统的能量。由上式，能量于拉格朗日函数的关系是线性的，由拉格朗日函数的可加性可以直接得出能量的可加性。
能量守恒不仅对于封闭系统成立，对位于定常外场(即不显含时间)中的系统也成立。能量守恒的力学系统也称保守系统。
由前面已知，封闭(或位于定常外场中的)系统的拉格朗日函数可以写成

$$L = T(q, \dot q) - U(q)$$

其中$T$是速度的二次函数。利用著名的齐次函数的欧拉定理可得

$$\dot q\frac{\partial L}{\partial \dot q} = \dot q\frac{\partial T}{\partial\dot q} = 2T$$

因此

$$E = T(q, \dot q) + U(q)$$

可见，系统的能量可以表示为本质不同的两项之和：依赖于速度的动能和仅依赖于质点坐标的势能。

### 动量

#### 封闭系统

根据空间均匀性，封闭力学系统在空间中整体平移时，其性质保持不变。因此我们研究一个无穷小平移$\vec\epsilon$，并求拉格朗日函数保持不变的条件。
平移就是将系统中所有质点移动相同的位移$\vec\epsilon$的变换，即矢径$\vec r_a \to \vec r_a + \vec\epsilon$。在速度不变时，坐标的无穷小的改变使拉格朗日函数产生的变化为

$$\delta L = \frac{\partial L}{\partial \vec r_a}\cdot\delta\vec r_a = \vec\epsilon\cdot\frac{\partial L}{\partial\vec r_a}$$

对任意$\vec\epsilon$要求$\delta L = 0$等价于

$$\frac{\partial L}{\partial\vec r_a} = 0$$

根据拉格朗日方程得

$$\frac{d}{dt}\frac{\partial L}{\partial\vec v_a} = 0$$

于是我们可得出结论：封闭力学系统得矢量

$$\vec P = \frac{\partial L}{\partial \vec v_a} = m_a\vec v_a$$

在运动中保持不变。矢量$\vec P$称为系统的动量。动量的可加性是显然的。与能量不同之处在于，无论质点之间的相互作用是否可以忽略，系统的动量都等于各个质点的动量之和。

导数$\partial L / \partial \vec r_a = -\partial U / \partial \vec r_a$是作用在第$a$个质点上的力$\vec F_a$。上述讨论表明，作用在封闭系统的所有质点上的力之和等于零：

$$\sum_a\vec F_a = 0$$

特别地，当系统只由两个质点组成时，$F_1 + F_2 = 0$，两个质点的相互作用力大小相等、方向相反。这就是著名的牛顿第三定律。

#### 非封闭系统

在没有外场的情况下，动量矢量的三个分量都守恒。然而在有外场的情况下，如果势能不显含某个笛卡尔坐标，则相应的该方向的动量分量守恒。显然，沿着这个不出现在势能中的坐标相应的坐标轴平移不会改变力学系统的性质，动量在该轴上投影守恒。例如，在方向沿着$z$轴的均匀场中，沿着$x$和$y$轴的动量分量守恒。

### 角动量

各向同性意味着封闭系统整体在空间中任意转动时，力学性质保持不变。因此，我们研究系统整体的无穷小转动并求出拉格朗日函数保持不变的条件。
我们引入无穷小转动矢量$\delta\vec\varphi$，其大小等于转角$\delta\varphi$，方向沿着转动轴(转动方向与$\delta\vec\varphi$的方向之间符合右手螺旋法则)。

![矢径端点的线位移与转角的关系](../assets/images/c2ec3a7999d241b48d77a5de42770883.png){loading=lazy}
/// caption
矢径端点的线位移与转角的关系
///

我们首先研究，在系统转动时，从坐标原点(位于转动轴上)指向系统中任意质点的矢径的位移。矢径端点的线位移与转角的关系为

$$|\delta\vec r| = r\sin\theta\cdot\delta\varphi$$

位移矢量的方向垂直过$\vec r$和$\delta\vec\varphi$的平面。显然有

$$\delta\vec r = \delta\vec\varphi \times \vec r$$

于是速度相对固定坐标系的增量为

$$\delta\vec v = \delta\vec\varphi \times \vec v$$

将这些表达式代入转动时拉格朗日函数不变的条件

$$\delta L = (\frac{\partial L}{\partial\vec r_a}\cdot \delta\vec r_a + \frac{\partial L}{\partial\vec v_a}\cdot \delta\vec v_a) = 0$$

并作代换$\partial L / \partial\vec v_a = \vec p_a, \partial L / \partial\vec r_a = \dot{\vec p_a}$，得

$$\dot{\vec p_a}\cdot(\delta\varphi \times \vec r_a) + \vec p_a\cdot(\delta\varphi \times \vec v_a) = 0$$

注意到混合积的轮换性，我们置换因子的次序并提出$\delta\varphi$，

$$\delta\varphi\cdot(\vec r_a\times\dot{\vec p_a} + \vec v_a \times\vec p_a) = \delta\varphi\cdot\frac{d}{dt}(\vec r_a \times \vec p_a) = 0$$

由$\delta\varphi$的任意性可得

$$\frac{d}{dt}(\vec r_a \times \vec p_a) = 0$$

即在封闭力学系统运动过程中矢量

$$\vec M = \sum_a\vec r_a \times \vec p_a$$

保持不变，这个物理量称为系统的角动量。类似于线动量，这个物理量不依赖于质点之间是否有相互作用，它的可加性是显然的。
