---
date:
  created: 2026-01-03
categories:
    - 数学
    - 算法
---

# 道理我都懂，但哈希表为什么这么快

## 从[两数之和](https://leetcode.cn/problems/two-sum/description/)说起

作为力扣上著名的第一题，该题的暴力做法需要$O(n^2)$枚举两个数，并判断其和是否为目标值。

<!-- more -->

``` cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        for (int i = 0; i < n; ++i)
            for (int j = i + 1; j < n; ++j)
                if (nums[i] + nums[j] == target)
                    return {i, j};
        return {};
    }
};
```
题目的进阶要求让我们想出一个时间复杂度小于$O(n^2)$的算法。一种想法是排序，复杂度是$O(n\log n)$。
``` cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        vector<int> idx(n);
        ranges::iota(idx, 0);
        ranges::sort(idx, {}, [&](int i){return nums[i];});
        int l = 0, r = n - 1;
        while (l < r) {
            int s = nums[idx[l]] + nums[idx[r]];
            if (s == target)
                return {idx[l], idx[r]};
            s < target ? ++l : --r;
        }
        return {};
    }
};
```
而形式上更为简洁的方式是使用哈希表存储值与下标的映射关系，这使得我们可以**平均**$O(1)$的时间找到与当前枚举的数字互补的那个数，从而以$O(n)$的复杂度完成此题。
``` cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (map.contains(target - nums[i]))
                return {i, map[target - nums[i]]};
            map[nums[i]] = i;
        }
        return {};
    }
};
```
为什么哈希表这么快？

## 哈希表的工作原理

在哈希表的内部，元素并不以有序的方式存储，而是存储在“桶”中。元素存储在哪个桶中完全取决于元素“键”的哈希值。拥有同样哈希值的键会出现在同一个桶中。这使得快速访问单个元素成为可能，因为一旦哈希值计算完毕，它就指向存储元素的桶。对于相等的键，我们要求哈希函数给出相同的哈希值，这被称为**一致性**。

不妨来看一下C++标准库中提供的哈希表相关函数，`bucket_count`返回哈希表所使用的桶的个数，而`max_bucket_count`返回系统支持的桶的个数上限。`bucket_size`返回指定桶的大小，`bucket`返回指定的键所对应的桶的索引。

在哈希表分析中，一个重要的指标是哈希表的**负载因子**，通常用$\alpha$表示。

\definition
    如果哈希表中共有$N$个元素，$M$个桶，则定义负载因子$\alpha = N / M$.

哈希表当前的负载因子可以通过函数`load_factor`取得。

### 均匀哈希假设

除了一致性，我们还希望哈希函数计算简单（**高效性**）并能均匀的映射所有的键（**均匀性**），即

!!! note "均匀哈希假设"
    我们使用的哈希函数能够均匀并独立地将所有的键映射到$0$到$M-1$之间。

毕竟，*hash*这个单词本身就有弄乱的意思。但研究表明，同时能够满足**一致性**，**高效性**和**均匀性**的哈希函数是不太可能的，因此均匀性只是一个假设（但是是至关重要的一个假设）。

有了上面这些铺垫，我们就可以更精确地分析哈希表的性能了，为此，我们首先指出

\proposition
    在一个有$M$个桶和$N$个键的哈希表中，（在均匀哈希假设前提下）**任意**一个桶中键的数量正比于$\alpha$的概率无限趋近于$1$.

\proof
    由二项分布可知，一个给定的桶恰好含有$k$个键的概率为

    $${N \choose k}\left (\frac{1}{M}\right )^k\left (\frac{M - 1}{M} \right )^{N - k} = {N \choose k}\left (\frac{\alpha}{N}\right )^k\left (1 - \frac{\alpha}{N} \right )^{N - k}$$

    二项分布（以及下面的泊松分布）期望为$\alpha$，说明桶中键的数量的平均值为$\alpha$个。对于较大的$N$，二项分布可由泊松分布近似（关于二项分布何时收敛到泊松分布，详见泊松定理）

    $$\frac{\alpha^k e^{-\alpha}}{k!}$$

    由此，一个桶中含有大于等于$t$个键的概率就是（当$t$很大时，使用等比数列近似和斯特林公式；或者参考Chernoff’s inequality）

    $$\sum_{k = t}^\infty\frac{\alpha^k e^{-\alpha}}{k!} \simeq \frac{\alpha^t e^{-\alpha}}{t!}\frac{t + 1}{t + 1 - \alpha} \simeq \frac{1}{\sqrt{2\pi t}}\left (\frac{e\alpha}{t}\right )^te^{-\alpha} < \left (\frac{e\alpha}{t}\right )^te^{-\alpha}$$

    这个界也称为Chernoff界，即一个桶中含有大于等于$T\alpha$个键的概率小于$(e/T)^{T\alpha}e^{-\alpha}$. （*所以Sedgewick算法书中的公式应该是错了？*） 于是对于任意的$\epsilon$，总可以找到一个$T$，使得桶中含有大于等于$T\alpha$个键的概率小于$\epsilon$. 这样桶中键的数量在$T\alpha$数量级的概率就无限趋近于$1$. 可以说明，即使不使用泊松近似，直接用二项分布进行精确计算，只要$T > 1$就能得出一样的结果。

由上面的分析，我们可以得出

\proposition
    在一个有$M$个桶和$N$个键的哈希表中，（在均匀哈希假设前提下）最坏情况下的访问操作需要的比较次数正比于$\alpha$

这表明，如果能保持哈希表中的桶数与表中的元素个数成正比，则哈希表单次操作可以在$O(\alpha) = O(1)$时间内完成。随着元素个数$N$的不断增加，如果不增加桶的数量$M$，$\alpha$就不再是常数。因此，哈希表会维护一个阈值，称为最大负载因子；当$\alpha$超过这个值的时候，哈希表会自动增加桶的数量，并对表中元素逐一重新做哈希映射。最大负载因子可以调用`max_load_factor`取得。C++也允许通过调用`rehash`手动触发这个操作。如果预先知道哈希表的元素上限，也可以调用`reserve`预留足够的桶，以确保不会使负载因子超过阈值而触发`rehash`。

### 好的哈希函数

从前面的分析可以看到，如果哈希函数选择地不好，使得函数值的分布不够均匀（甚至出现聚集），那么哈希表的性能就会退化。最差情况所有元素都聚集到一个桶里，时间复杂度会增加到$O(N)$. 实践中尽量保持哈希函数均匀性的方法如下：

#### 整数

对于整数通常采取取模的方法。选择桶的个数为素数$M$，对于任意整数$k$，可以计算$k$除以$M$的余数。如果$M$不是素数，就可能无法利用键种包含的所有信息。例如，如果键是十进制数而$M$为$10^k$，那么只有键的后$k$位会对哈希值产生影响。

#### 浮点数

通常采用取浮点数的二进制表示，然后取模。

#### 字符串

字符串无非就是128/256进制的整数，Horner算法可以用$N$次乘法、加法和取模计算一个字符串的哈希值，这个方法也被称为[滚动哈希](https://leetcode.cn/problem-list/rolling-hash/)/多项式哈希。

#### 结构体

与字符串的Horner算法类似，可以找一个合适的进制$R$然后进行滚动哈希。

## 后记

更多哈希函数相关题目，可参考[哈希函数题单](https://leetcode.cn/problem-list/hash-function/)。

本文只考虑了C++标准库中用到的链式实现，对于至少是同等重要的**开放地址**哈希表，读者可以自行查找相关资料进行了解。实际上，即使单看C++标准库的实现，也用到了其他的方法来防止哈希表的性能落入最坏情况，比如在桶中元素超过一定长度时改为二叉树存储等。由于本文目的仅为对哈希表的底层原理做一简要介绍，为免更多暴露作者的无知，不妨就到此结束吧。
