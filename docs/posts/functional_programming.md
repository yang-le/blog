---
date:
  created: 2026-10-23
categories:
    - 数学
---

# Functional Programming

## Category

范畴由**对象**和**态射**构成，态射是对象之间的箭头。若$X, Y$是对象，记$f : X \to Y$为从$X$到$Y$的态射。
如果$X, Y$之间存在态射$f$，$Y, Z$之间存在态射$g$，那么$X, Z$之间也必须存在一个态射，记作$g \circ f$，称为$g$和$f$的复合。

<!-- more -->

![态射的复合](https://yang-le.github.io/blog/assets/images/Commutative_diagram_for_morphism.svg)

态射需要满足以下性质

- (结合律) 对$f : W \to X, g : X \to Y$和$h : Y \to Z$，有$h \circ (g \circ f) = (h \circ g) \circ f$
- (单位元) 对任意对象$X$，须存在一个态射$1_X : X \to X$，使得对任意$f : Y \to Z$，有$1_Y \circ f = f = f \circ 1_Z$

## Functor

设$C$和$D$是两个范畴，从$C$到$D$的函子$F$是满足如下性质的映射

- 把$C$中的每一个对象$X$映射为$D$中的$F(X)$
- 把$C$中的每一个态射$f : X \to Y$映射为$D$中的态射$F(f) : F(X) \to F(Y)$，满足
    - $F(1_X) = 1_{F(X)}$
    - $F(g \circ f) = F(g) \circ F(f)$

换言之，函子保持单位态射以及态射的复合。

![alt text](https://yang-le.github.io/blog/assets/images/Commutative_diagram_of_a_functor.svg)

## Natrual Transform

设$F, G$都是从范畴$C$到$D$的函子，从$F$到$G$的自然变换$\eta : F \Rightarrow G$是一族态射$\eta_X$，其中$\eta_X : F(X) \to G(X)$称为$\eta$在$X$处的分量。此分量须满足对$C$中的每一个态射$f : X \to Y$，有$\eta_Y \circ F(f) = G(f) \circ \eta_X$.

![alt text](https://yang-le.github.io/blog/assets/images/Natural_Transformation_between_two_functors.svg)

## 函子

编程语言中的范畴包含所有类型和类型间的函数，其中类型是范畴中的对象，函数是范畴中的态射。函子是此范畴中的**自函子**。

``` lean
class Functor (f : Type u → Type v) where
  map : {α β : Type u} → (α → β) → f α → f β
```

上面的代码中，`f`是一个类型构造器（类似于泛型容器），它负责把对象`α : Type u`映射为另一个对象`f α : Type v`。`map`把对象间的函数`α → β`射为容器间的函数`f α → f β`。

语法糖`g <$> x`使用`map`映射普通函数`g : α → β`，并将其结果应用到容器中的元素`x : f α`上，得到`f β`中的另一个元素。函子保持单位态射以及态射的复合的性质可表述为

- `id <$> x = x`
- `(g ∘ f) <$> x = g <$> f <$> x`

## 应用函子

### 张量（幺半群）范畴

幺半群范畴是具有幺半群结构的范畴$C$，具体来说包含：

- 一个二元函子$\otimes : C \times C \to C$，称为张量积
- 一个单位对象$I$
- 三个自然变换（同构）
    - 结合子$\alpha_{A, B, C} : (A \otimes B) \otimes C \simeq A \otimes (B \otimes C)$
    - 左单位子$\lambda_A : I \otimes A \simeq A$
    - 右单位子$\rho_A : A \otimes I \simeq A$

且满足如下条件
![- 五边形交换图](https://yang-le.github.io/blog/assets/images/Pentagonal_diagram_for_monoidal_categories.svg)
![三角形交换图](https://yang-le.github.io/blog/assets/images/Monoidal2.svg)

### 幺半群函子

幺半群函子即幺半群范畴之间的保持幺半群结构的函子。具体来说，两个幺半群范畴之间的函子包含函子本身，以及两个一致性映射：一个保持张量积的自然变换和一个保持单位对象的态射。对这两个一致性映射的不同要求产生了幺半群函子的不同版本：

- （松弛）幺半群函子；没有额外要求
- 强幺半群函子；要求一致性映射可逆（同构）
- 严格幺半群函子；要求一致性映射恒等

#### 松弛幺半群函子

设$(\mathcal C, \otimes, I_\mathcal C)$和$(\mathcal D, \bull, I_\mathcal D)$是两个幺半群范畴。从$\mathcal C$到$\mathcal D$的（松弛）幺半群函子包括一个函子$F : \mathcal C \to \mathcal D$以及

- 一个（函子$\mathcal D \times \mathcal D \to \mathcal C$之间的）自然变换$\phi_{A, B} : F A \bull F B \to F (A \otimes B)$
- 一个态射$\phi : I_{\mathcal D} \to F I_{\mathcal C}$

满足如下$\mathcal D$中的三个交换图

![alt text](https://yang-le.github.io/blog/assets/images/Lax_monoidal_functor_associative.svg)
![alt text](https://yang-le.github.io/blog/assets/images/Lax_monoidal_functor_right_unit.svg)
![alt text](https://yang-le.github.io/blog/assets/images/Lax_monoidal_functor_left_unit.svg)


### 张量强度

给定幺半群范畴$(\mathcal C, \otimes, I_\mathcal C)$，函子$F : \mathcal C \to \mathcal C$的张量强度通常定义为自然变换$\beta_{c, a} : c \otimes Fa \to F (c \otimes a)$

它满足的交换图见 https://ncatlab.org/nlab/show/tensorial+strength

可以把$c$想象成运算$F a$所需的上下文，强度意味着我们可以在运算中使用外部上下文。

### `seq`方法

``` lean
class Seq (f : Type u → Type v) : Type (max (u+1) v) where
  seq : {α β : Type u} → f (α → β) → (Unit → f α) → f β
```

`f (α → β) → f α` --使用$\phi$--> `f ((α → β) × α)` --使用`map eval`--> `f β`

### `pure`方法

``` lean
class Pure (f : Type u → Type v) : Type (max (u+1) v) where
  pure {α : Type u} : α → f α
```

`α` --使用$\rho_\alpha^{-1}$--> `α × ()` --使用$\phi$--> `α × f ()` --使用$\beta_{\alpha, I}$ --> `f (α × ())` --使用`map`$\rho_\alpha$--> `f α`

`pure`是自然变换$I \Rightarrow F$，其中$I$是恒等函子；`pure {α}`是态射$\alpha \to F \alpha$，因此需要满足`g <$> pure x = pure (g x)`.

实际上，这样的要求有四条，上面说明的是`map_pure`.

``` lean
pure_seq {α β : Type u} (g : α → β) (x : f α) : pure g <*> x = g <$> x
map_pure {α β : Type u} (g : α → β) (x : α) : g <$> pure x = pure (g x)
seq_pure {α β : Type u} (g : f (α → β)) (x : α) : g <*> pure x = (fun (h : α → β) => h x) <$> g
seq_assoc {α β γ : Type u} (x : f α) (g : f (α → β)) (h : f (β → γ)) : h <*> (g <*> x) = Function.comp <$> h <*> g <*> x
```

`seq_assoc`对应松弛幺半群函子的第一个交换图，即结合律。`h <*> (g <*> x)`对应图的右半边路径，`Function.comp <$> h <*> g <*> x`对应图的左半边路径（按从左到右的顺序结合，使用`<$>`把`Function.comp`送进去，让`h`和`g`先结合）。

`pure_seq`对应松弛幺半群函子的左单位元交换图，`seq_pure`对应右单位元交换图。

## 单子

范畴$C$上的单子包括一个自函子$T : C \to C$以及两个自然变换$\eta : 1_C \to T$和$\mu : T^2 \to T$，且它们满足如下条件

- $\mu \circ T \mu = \mu \circ \mu T$
- $\mu \circ T \eta = \mu \circ \eta T = 1_T$

![alt text](https://yang-le.github.io/blog/assets/images/Monad_multiplication_explicit.svg)
![alt text](https://yang-le.github.io/blog/assets/images/Monad_unit_explicit.svg)

等价地，单子是自函子范畴中的幺半群。

### `bind`方法

``` lean
class Bind (m : Type u → Type v) where
  bind : {α β : Type u} → m α → (α → m β) → m β
```

`bind ma g`实际上是`μ (g <$> ma)`。`g <$> ma`的结果是`m (m β)`，再经过`μ`就变为`m β`。它同样需要满足四条性质

``` lean
bind_pure_comp (f : α → β) (x : m α) : x >>= (fun a => pure (f a)) = f <$> x
bind_map       {α β : Type u} (f : m (α → β)) (x : m α) : f >>= (. <$> x) = f <*> x
pure_bind      (x : α) (f : α → m β) : pure x >>= f = f x
bind_assoc     (x : m α) (f : α → m β) (g : β → m γ) : x >>= f >>= g = x >>= fun x => f x >>= g
```

`pure_bind`左边是`μ (f <$> (pure x)) = μ (pure (f x))`，因此对应$\mu \circ \eta T = 1_T$.

`bind_assoc`左边是`μ (g <$> (μ (f <$> x)))`相当于$\mu \circ \mu T$，右边是`μ (fun x => f x >>= g <$> x)`相当于$\mu \circ T \mu$

`bind_map`说明如何使用`bind`实现`seq`方法，体现了`Monad`和`Applicative`的关系。

`bind_pure_map`说明如何使用`bind`实现`map`方法，体现了``Monad``和``Functor``的关系。

所以$\mu \circ T \eta = 1_T$在哪呢？也就是说`x >>= pure = x`即`μ (map pure x) = x`如何体现呢？很简单，根据`bind_pure_comp`，令`f = id`，`x`任意，就有`x >>= pure = id <$> x = x`。
