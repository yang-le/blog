---
date:
  created: 2026-08-05
categories:
    - 算法
---

# 数位DP

数位DP用于统计给定区间内满足一定条件的数的数量，区间通常很大（比如$10^{18}$），一般方法会超时。

<!-- more -->

数位DP将候选的数字分成数位，从高位到低位枚举每一位上可以填哪些数字。设$f(i)$表示从第$i$位开始填数字对应的结果数量。那么枚举第$i$位所有可能填的数字$d$，有

$$f(i) = \sum_d f(i + 1)$$

边界条件是当$i = n$时，此时所有数位都已确定，需要根据题目要求判断对应的数字是否满足条件，对应地返回$1$或$0$即可。题目要求的即$f(0)$。

如何确定$d$的取值范围？如果前面的所有数位都已触及给定区间上界对应的数位，则$d$最多只能取到区间上界对应的当前数位；否则就可以取到$9$。

$d$最小可以取到多少？$0$吗？未必，这取决于我们填的是否是这个数字的第$0$位（最高位），最高位不能是$0$，而应该从$1$开始。

此外，对于前缀位，我们还可以选择不填数字（对应位数小于上界位数的情况），这种情况下递推方程就变为$f(i) = f(i + 1)$的形式。

一般来说，数位DP的代码具有如下的形式

``` cpp
auto dfs = [&](this auto&& dfs, int i, bool is_limit, bool is_num) -> int {
    if (i == n)
        // (1)!
        return 1;
    
    int ans = 0;
    if (!is_num)
        // (2)!
        ans = dfs(i + 1, false, false);

    int up = is_limit ? s[i] - '0' : 9; // (3)!
    for (int d = is_num ? 0 : 1; d <= up; ++d) // (4)!
        ans += dfs(i + 1, is_limit && d == up, true); // (5)!

    return ans;
};
return dfs(0, true, false);
```

1. 所有数位都已确定。
    
    如果此时is_num为false，表示我们跳过了所有的数位，这种情况可以看作是0

2. 还未开始填入数字，因此可以选择不填。

    对应于这种位数小于上界位数的情况，无需限制首位的范围，因此is_limit置为false

3. 已经触及上界就只能填到s[i] - '0'，否则能够填到9
4. 如果已经开始填数则可以从0开始枚举，否则只能从1开始枚举
5. 按转移方程继续。
    
    若前面的所有数位都已触及上界，且当前数位也已触及上界，则置is_limit为true；否则为false

其中`is_limit`表示前面的所有数位是否都已触及给定区间的上界对应的数位，初始为`true`，因为我们必须限制最高位不能超过上界的最高位。`is_num`表示我们是否已经开始填数（注意前缀位有可能选择跳过），初始为`false`，因为我们还未填入任何数字。

这段代码对应的区间范围是$[0, s]$，如果题目要求的区间为$[l, r]$，则分别计算$[0, r]$和$[0, l - 1]$的结果，两者相减即可。

例题：

- [统计特殊整数](https://leetcode.cn/problems/count-special-integers/description/)
- [最大为 N 的数字组合](https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/description/)

    数位DP的用途并不限于十进制，比如下面这个题的数据范围很小（不超过六位数），但使用数位DP可以轻松处理十几位甚至几十位的数字。

- [二进制表示中质数个计算置位](https://leetcode.cn/problems/prime-number-of-set-bits-in-binary-representation/description/)

=== "统计特殊整数"

    ``` cpp
    class Solution {
    public:
        int countSpecialNumbers(int n) {
            string s = to_string(n);
            int m = s.size();
            vector memo(m, vector<int>(1024, -1));
            auto dfs = [&](this auto&& dfs, int i, int mask, bool is_limit, bool is_num) -> int {
                if (i == m)
                    return is_num;
                
                if (!is_limit && is_num && memo[i][mask] != -1)
                    return memo[i][mask];
                
                int ans = 0;
                if (!is_num)
                    ans = dfs(i + 1, 0, false, false);
                
                int up = is_limit ? s[i] - '0' : 9;
                for (int d = is_num ? 0 : 1; d <= up; ++d)
                    if (!(mask >> d & 1))
                        ans += dfs(i + 1, mask | (1 << d), is_limit && d == up, true);
                
                if (!is_limit && is_num)
                    memo[i][mask] = ans;
                
                return ans;
            };
            return dfs(0, 0, true, false);
        }
    };
    ```

=== "最大为 N 的数字组合"

    ``` cpp
    class Solution {
    public:
        int atMostNGivenDigitSet(vector<string>& digits, int n) {
            string s = to_string(n);
            int m = s.size();
            vector memo(m, -1);
            auto dfs = [&](this auto&& dfs, int i, bool is_limit, bool is_num) -> int {
                if (i == m)
                    return is_num;
                
                if (!is_limit && is_num && memo[i] != -1)
                    return memo[i];
                
                int ans = 0;
                if (!is_num)
                    ans = dfs(i + 1, false, false);
                
                char up = is_limit ? s[i] : '9';
                for (string& d : digits) {
                    if (d[0] > up)
                        break;
                    ans += dfs(i + 1, is_limit && d[0] == up, true);
                }
                
                if (!is_limit && is_num)
                    memo[i] = ans;
                
                return ans;
            };
            return dfs(0, true, false);
        }
    };
    ```

=== "二进制表示中质数个计算置位"

    ``` cpp
    bool isPrime(int n) {
        if (n < 2)
            return false;
        for (int x = 2; x * x <= n; ++x)
            if (n % x == 0)
                return false;
        return true;
    }

    class Solution {
    public:
        int countPrimeSetBits(int left, int right) {
            auto calc = [](unsigned x) -> int {
                int m = bit_width(x);
                vector memo(m, vector<int>(m, -1));
                auto dfs = [&](this auto&& dfs, int i, int cnt, bool is_limit, bool is_num) -> int {
                    if (i == -1)
                        return isPrime(cnt);
                    
                    if (!is_limit && is_num && memo[i][cnt] != -1)
                        return memo[i][cnt];
                    
                    int ans = 0;
                    if (!is_num)
                        ans = dfs(i - 1, 0, false, false);
                    
                    int up = is_limit ? (x >> i & 1) : 1;
                    for (int d = is_num ? 0 : 1; d <= up; ++d)
                        ans += dfs(i - 1, cnt + (d == 1), is_limit && d == up, true);
                    
                    if (!is_limit && is_num)
                        memo[i][cnt] = ans;
                    
                    return ans;
                };
                return dfs(m - 1, 0, true, false);
            };
            return calc(right) - calc(left - 1);
        }
    };
    ```

## 上下界数位DP

有时候下界减一并不好算（比如下界是以字符串给出的，且可能很大，无法转为`long long`），实际上可以改进上面的模板，使其可以处理非零的下界。

进一步地，可以去掉`is_num`这个参数，并真正地将前导零和数字$0$区分开。

=== "上下界数位DP其一"

    ``` cpp
    string low, high;
    int n = high.size();
    low = string(n - low.size(), '0') + low; // (1)!

    auto dfs = [&](this auto&& dfs, int i, bool limit_low, bool limit_high, bool is_num) -> int {
        if (i == n)
            // (2)!
            return 1;
        
        int ans = 0;
        if (!is_num && low[i] == '0')
            // (3)!
            ans = dfs(i + 1, true, false, false);
        
        int lo = limit_low ? low[i] - '0' : 0; // (4)!
        int hi = limit_high ? high[i] - '0' : 9; // (5)!
        int d0 = is_num ? 0 : 1; // (6)!
        for (int d = max(d0, lo); d <= hi; ++d)
            // (7)!
            ans += dfs(i + 1, limit_low && d == lo, limit_high && d == hi, true);
        
        return ans;
    };
    return dfs(0, true, true, false);
    ```

    1. `low`要通过补齐前导零和`high`对齐
    1. 所有数位都已确定

        如果此时`is_num`为`false`，表示我们跳过了所有的数位，这种情况可以看作是`0`

    1. 还未开始填入数字，因此可以选择不填

        此时`limit_low`一定是`true`，因为都是`0`

        对应于这种位数小于上界位数的情况，无需限制首位的范围，因此`limit_high`置为`false`

    1. 已经触及下界就只能从`low[i] - '0'`开始填，否则能够从`0`开始填
    1. 已经触及上界就只能填到`high[i] - '0'`，否则能够填到`9`
    1. 如果已经开始填数则可以从`0`开始枚举，否则只能从`1`开始枚举
    1. 按转移方程继续

        若前面的所有数位都已触及下界，且当前数位也已触及下界，则置`limit_low`为`true`；否则为`false`

        若前面的所有数位都已触及上界，且当前数位也已触及上界，则置`limit_high`为`true`；否则为`false`

=== "上下界数位DP其二"

    ``` cpp
    string low, high;
    int n = high.size();
    int diff_lh = n - low.size();

    auto dfs = [&](this auto&& dfs, int i, bool limit_low, bool limit_high) -> int {
        if (i == n)
            // (1)!
            return 1;

        int lo = limit_low && i >= diff_lh ? low[i - diff_lh] - '0' : 0; // (2)!
        int hi = limit_high ? high[i] - '0' : 9; // (3)!

        int d = lo;
        int ans = 0;

        // (4)!
        if (limit_low && i < diff_lh) {
            // (5)!
            ans = dfs(i + 1, true, false);
            d = 1; // (6)!
        }

        for (; d <= hi; ++d)
            // (7)!
            ans += dfs(i + 1, limit_low && d == lo, limit_high && d == hi);
        
        return ans;
    };
    return dfs(0, true, true);
    ```

    1. 所有数位都已确定
    2. 已经触及下界就只能从`low[i - diff_lh] - '0'`开始填，否则能够从`0`开始填
    3. 已经触及上界就只能填到`high[i] - '0'`，否则能够填到`9`
    4. 如果将前导零计算在内也不影响答案，可以去掉这个`if`块
    5. 还未开始填入数字，因此可以选择不填

        对应于这种位数小于上界位数的情况，无需限制首位的范围，因此`limit_high`置为`false`

    6. 下面填数字，只能从`1`开始填（最高位不能是`0`）
    7. 按转移方程继续
    
        若前面的所有数位都已触及下界，且当前数位也已触及下界，则置`limit_low`为`true`；否则为`false`

        若前面的所有数位都已触及上界，且当前数位也已触及上界，则置`limit_high`为`true`；否则为`false`


例题：

- [统计强大整数的数目](https://leetcode.cn/problems/count-the-number-of-powerful-integers/description/)
- [统计整数数目](https://leetcode.cn/problems/count-of-integers/description/)
- [统计美丽整数的数目](https://leetcode.cn/problems/count-beautiful-numbers/description/)

=== "统计强大整数的数目"

    ``` cpp
    class Solution {
    public:
        long long numberOfPowerfulInt(long long start, long long finish, int limit, string s) {
            string low = to_string(start), high = to_string(finish);
            int n = high.size();
            int diff_lh = n - low.size();
            int diff_sh = n - s.size();

            vector<long long> memo(n, -1);
            auto dfs = [&](this auto&& dfs, int i, bool limit_low, bool limit_high) -> long long {
                if (i == n)
                    return 1;

                if (!limit_low && !limit_high && memo[i] != -1)
                    return memo[i];

                int lo = limit_low && i >= diff_lh ? low[i - diff_lh] - '0' : 0;
                int hi = limit_high ? high[i] - '0' : 9;

                int d = lo;
                long long ans = 0;

                if (i >= diff_sh) {
                    d = s[i - diff_sh] - '0'; // (1)!
                    if (lo <= d && d <= hi)
                        ans += dfs(i + 1, limit_low && d == lo, limit_high && d == hi);
                } else 
                    for (; d <= min(limit, hi); ++d)
                        ans += dfs(i + 1, limit_low && d == lo, limit_high && d == hi);
                
                if (!limit_low && !limit_high)
                    memo[i] = ans;

                return ans;
            };
            return dfs(0, true, true);
        }
    };
    ```

    1. 只能填`s[i - diff_sh] - '0'`

=== "统计整数数目"

    ``` cpp
    class Solution {
    public:
        int count(string num1, string num2, int min_sum, int max_sum) {
            int n = num2.size();
            int diff_lh = n - num1.size();

            vector memo(n, vector<int>(min(n * 9, max_sum) + 1, -1));
            auto dfs = [&](this auto&& dfs, int i, int sum, bool limit_low, bool limit_high) -> int {
                if (sum > max_sum)
                    return 0;
                if (i == n)
                    return min_sum <= sum;

                if (!limit_low && !limit_high && memo[i][sum] != -1)
                    return memo[i][sum];

                int lo = limit_low && i >= diff_lh ? num1[i - diff_lh] - '0' : 0;
                int hi = limit_high ? num2[i] - '0' : 9;

                int d = lo;
                int ans = 0;

                for (; d <= hi; ++d)
                    ans = (ans + dfs(i + 1, sum + d, limit_low && d == lo, limit_high && d == hi)) % 1'000'000'007;
                
                if (!limit_low && !limit_high)
                    memo[i][sum] = ans;

                return ans;
            };
            return dfs(0, 0, true, true);
        }
    };
    ```

=== "统计美丽整数的数目"

    ``` cpp
    class Solution {
    public:
        int beautifulNumbers(int l, int r) {
            string low = to_string(l), high = to_string(r);
            int n = high.size();
            int diff_lh = n - low.size();

            vector memo(n, vector<unordered_map<int, int>>(n * 9 + 1));
            auto dfs = [&](this auto&& dfs, int i, int sum, int prod, bool limit_low, bool limit_high) -> int {
                if (i == n)
                    return prod % sum == 0;

                if (!limit_low && !limit_high && memo[i][sum].contains(prod))
                    return memo[i][sum][prod];

                int lo = limit_low && i >= diff_lh ? low[i - diff_lh] - '0' : 0;
                int hi = limit_high ? high[i] - '0' : 9;

                int d = lo;
                int ans = 0;

                if (limit_low && i < diff_lh) {
                    ans = dfs(i + 1, 0, 1, true, false);
                    d = 1;
                }

                for (; d <= hi; ++d)
                    ans += dfs(i + 1, sum + d, prod * d, limit_low && d == lo, limit_high && d == hi);
                
                if (!limit_low && !limit_high)
                    memo[i][sum][prod] = ans;

                return ans;
            };
            return dfs(0, 0, 1, true, true);
        }
    };
    ```
