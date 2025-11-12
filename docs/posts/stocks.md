---
date:
  created: 2025-11-12
categories:
    - 数学
    - 算法
---

# 如何科学地买卖股票

> 本文不构成任何买入或卖出股票的建议，作者亦不对读者的股市收益负有任何（法律或非法律的）责任。
> 股市有风险，入市需谨慎。

<!-- more -->

## [买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock)

假设第$i$天的股票价格是$p_i$，我们只需要知道**历史最低**价格，即可求出**最大**利润。

``` cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int ans = 0;
        int low = prices[0];

        for (auto p : prices) {
            ans = max(ans, p - low);
            low = min(low, p);
        }

        return ans;
    }
};
```

## [买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii)

现在要求同一天只能持有最多一支股票，我们可以分别考虑第$i$天持有或不持有股票的情况。设$f_0(i),f_1(i)$分别为第$i$天不持有和持有股票所能获得的最大利润，显然

$$f_0(0) = 0, f_1(0) = -p_0$$

这是因为第$0$天不持有股票的利润是$0$，而如果想要第$0$天持有股票，则需要花费$p_0$购买，因此利润为负。

第$i + 1$天的利润和第$i$天的利润有如下的关系

$$\begin{align*}
f_0(i + 1) = \max(f_0(i), f_1(i) + p_{i + 1}) \\
f_1(i + 1) = \max(f_1(i), f_0(i) - p_{i + 1})
\end{align*}$$

即第$i + 1$天不持有股票分两种情况，要么前一天本就不持有股票，要么前一天持有股票然后今天卖掉，取二者能获得利润的最大值。注意

1. 前一天不持有股票今天买入
2. 前一天持有股票今天继续持有

这两种操作是不会获得最大利润的，即

$$\max(f_0(i), f_0(i) - p_{i + 1}, f_1(i), f_1(i) + p_{i + 1}) = \max(f_0(i), f_1(i) + p_{i + 1})$$

第$i + 1$天持有股票的情况可以类似分析。

现在来到最后一天，不难理解，我们需要在当天把手中的股票全卖掉才能获得最大利润，因此$f_0(n - 1)$即是我们所求。

``` cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buy = -prices[0], sell = 0, n = prices.size();
        for (int i = 1; i < n; ++i) {
            int temp = buy;
            buy = max(buy, sell - prices[i]);
            sell = max(sell, temp + prices[i]);
        }
        return sell;
    }
};
```

### [买卖股票的最佳时机 含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee)

如果要把手续费考虑在内，只需在计算收益时减去手续费即可。

``` cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int buy = -prices[0], sell = 0, n = prices.size();
        for (int i = 1; i < n; ++i) {
            int temp = buy;
            buy = max(buy, sell - prices[i]);
            sell = max(sell, temp + prices[i] - fee);
        }
        return sell;
    }
};
```

### [买卖股票的最佳时机 含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown)

如果要求包含一天的冷冻期，即卖出股票后的第二天无法买入，只需修改上面持有股票时的递推关系为

$$f_1(i + 1) = \max(f_1(i), f_0(i - 1) - p_{i + 1})$$

即第$i + 1$天如果想买入的话，第$i$天一定不能卖出（即要么第$i$天没有操作，和第$i - 1$天的情况一样；要么第$i$天买入了，其最大收益小于第$i - 1$天），于是可以在第$i - 1$天的最大收益基础上计算。另外要注意$i < 0$时$f_0(i) = 0$.

``` cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<int> memo[2] = {vector<int>(n, INT_MIN), vector<int>(n, INT_MIN)};
        memo[0][0] = 0;
        memo[1][0] = -prices[0];

        auto dfs = [&](this auto&& dfs, int i, bool hold) {
            if (i < 0)
                return 0;

            int& ans = memo[hold][i];
            if (ans != INT_MIN)
                return ans;
            
            if (hold)
                return ans = max(dfs(i - 1, true), dfs(i - 2, false) - prices[i]);
            return ans = max(dfs(i - 1, false), dfs(i - 1, true) + prices[i]);
        };
        return dfs(n - 1, false);
    }
};
```

## [买卖股票的最佳时机 III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii)

现在限制最多可以完成两笔交易，这是下节内容在$k = 2$的特殊情况，具体请见下节。

## [买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv)

现在限制最多可以完成$k$笔交易。我们在[买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii)的基础上，再增加一个参数$k$表示剩余的交易次数。则当$k > 0$或持有股票（即未交易，不消耗$k$）时，递推关系是类似的

$$\begin{align*}
f_0(i + 1, k) &= \max(f_0(i, k), f_1(i, k - 1) + p_{i + 1}) \\
f_1(i + 1, k) &= \max(f_1(i, k), f_0(i, k) - p_{i + 1})
\end{align*}$$

当$k = 0$时，我们不能再交易，因此

$$f_0(i + 1, 0) = f_0(i, 0)$$

$f_0(n - 1, k)$即是我们所求。

``` cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> memo[2] = {vector(k + 1, vector(n, INT_MIN)), vector(k + 1, vector(n, INT_MIN))};
        for (int i = 0; i <= k; ++i) {
            memo[0][i][0] = 0;
            memo[1][i][0] = -prices[0];
        }
        auto dfs = [&](this auto&& dfs, int i, bool hold, int k) {
            int& ans = memo[hold][k][i];
            if (ans != INT_MIN)
                return ans;

            if (hold)
                return ans = max(dfs(i - 1, true, k), dfs(i - 1, false, k) - prices[i]);
            if (k > 0)
                return ans = max(dfs(i - 1, false, k), dfs(i - 1, true, k - 1) + prices[i]);
            return ans = dfs(i - 1, false, 0);
        };
        return dfs(n - 1, false, k);
    }
};
```

## [买卖股票的最佳时机 V](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-v)

引入**做空交易**，即允许先卖后买，利润仍是卖出价格减买入价格。仍然有$k$笔交易的限制，这可以通过在[买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv)的基础上增加做空交易的类型来实现。定义$f_2(i, k)$表示剩余$k$次交易的情况下，第$i$天进行做空交易的最大获利。递推关系需要增加一条

$$\begin{align*}
f_0(i + 1, k) &= \max(f_0(i, k), f_1(i, k - 1) + p_{i + 1}, f_2(i, k - 1) - p_{i + 1}) \\
f_1(i + 1, k) &= \max(f_1(i, k), f_0(i, k) - p_{i + 1}) \\
f_2(i + 1, k) &= \max(f_2(i, k), f_0(i, k) + p_{i + 1})
\end{align*}$$

所求仍是$f_0(n - 1, k)$.

``` cpp
class Solution {
public:
    long long maximumProfit(vector<int>& prices, int k) {
        int n = prices.size();
        vector<vector<long long>> memo[3] = {vector(k + 1, vector(n, LLONG_MIN)), vector(k + 1, vector(n, LLONG_MIN)), vector(k + 1, vector(n, LLONG_MIN))};
        for (int i = 0; i <= k; ++i) {
            memo[0][i][0] = 0;
            memo[1][i][0] = -prices[0];
            memo[2][i][0] = prices[0];
        }
        auto dfs = [&](this auto&& dfs, int i, int type, int k) {
            long long& ans = memo[type][k][i];
            if (ans != LLONG_MIN)
                return ans;

            if (type == 1)
                return ans = max(dfs(i - 1, 1, k), dfs(i - 1, 0, k) - prices[i]);
            if (type == 2)
                return ans = max(dfs(i - 1, 2, k), dfs(i - 1, 0, k) + prices[i]);
            if (k > 0)
                return ans = max({dfs(i - 1, 0, k), dfs(i - 1, 1, k - 1) + prices[i], dfs(i - 1, 2, k - 1) - prices[i]});
            return ans = dfs(i - 1, 0, 0);
        };
        return dfs(n - 1, 0, k);
    }
};
```

### 股票交易小知识

- **做多** 即先买后卖（低买高卖），买入时现金减少，卖出时现金增加，买入 - 卖出算一次完整交易，平仓时消耗一次交易次数。
- **做空** 即先卖后买（高卖低买），卖出时现金增加，买回时现金减少，卖出 - 买回算一次完整交易，平仓时消耗一次交易次数。
- **持仓状态** 分为空仓，持有做多仓位，持有做空仓位三种状态。最后一天必须**平仓**（即不持有仓位），此时非空仓状态的利润将被视为无效。交易次数耗尽后不能开新仓。
