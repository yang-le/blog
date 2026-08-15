---
date:
  created: 2026-08-15
categories:
    - 算法
---

# 单调栈

单调栈是栈这种数据结构的一个应用，因栈中的数据始终保持单调性而得名。典型应用是求数组中每个元素的下一个更大元素出现的位置。

<!-- more -->

例题：

[每日温度](https://leetcode.cn/problems/daily-temperatures/description/)

=== "从左往右遍历"

    ``` cpp
    class Solution {
    public:
        vector<int> dailyTemperatures(vector<int>& temperatures) {
            int n = temperatures.size();
            vector<int> ans(n);
            stack<int> st;
            for (int i = 0; i < n; ++i) {
                while (!st.empty() && temperatures[i] > temperatures[st.top()]) {
                    // 当前温度如果高于栈顶，则说明当前温度就是栈顶元素的下一个更高温度
                    ans[st.top()] = i - st.top();
                    // 栈顶元素的答案已经得出
                    st.pop();
                }
                st.push(i);
            }
            return ans;
        }
    };
    ```

=== "从右往左遍历"

    ``` cpp
    class Solution {
    public:
        vector<int> dailyTemperatures(vector<int>& temperatures) {
            int n = temperatures.size();
            vector<int> ans(n);
            stack<int> st;
            for (int i = n - 1; i >= 0; --i) {
                while(!st.empty() && temperatures[i] >= temperatures[st.top()])
                    // 倒着遍历，如果当前温度比之前遍历的温度高，说明之前的温度不可能作为下一个更高温度
                    // 因此可以弹出
                    st.pop();
                if (!st.empty())
                    // 此时，栈顶的元素就是下一个更高温度
                    ans[i] = st.top() - i;
                st.push(i);
            }
            return ans;
        }
    };
    ```

来看示例1:

输入是 `[73, 74, 75, 71, 69, 72, 76, 73]`

=== "从左往右遍历"

    ``` mermaid
    graph LR
        S0["初始<br>─────<br>[ ]"]
        S1["① Push 73<br>─────<br>73"]
        S2["② Pop<br>─────<br>[ ]"]
        S3["③ Push 74<br>─────<br>74"]
        S4["④ Pop<br>─────<br>[ ]"]
        S5["⑤ Push 75<br>─────<br>75"]
        S6["⑥ Push 71<br>─────<br>71<br>75"]
        S7["⑦ Push 69<br>─────<br>69<br>71<br>75"]
        S8["⑧ Pop<br>─────<br>71<br>75"]
        S9["⑨ Pop<br>─────<br>75"]
        S10["⑩ Push 72<br>─────<br>72<br>75"]
        S11["⑪ Pop<br>─────<br>75"]
        S12["⑫ Pop<br>─────<br>[ ]"]
        S13["⑬ Push 76<br>─────<br>76"]
        S14["⑭ Push 73<br>─────<br>73<br>76"]

        %% S0 --> S1 --> S2

        style S0 fill:#e3f2fd
        style S1 fill:#c8e6c9
        style S2 fill:#ffcdd2
        style S3 fill:#c8e6c9
        style S4 fill:#ffcdd2
        style S5 fill:#c8e6c9
        style S6 fill:#c8e6c9
        style S7 fill:#c8e6c9
        style S8 fill:#fff9c4
        style S9 fill:#fff9c4
        style S10 fill:#c8e6c9
        style S11 fill:#fff9c4
        style S12 fill:#ffcdd2
        style S13 fill:#c8e6c9
        style S14 fill:#c8e6c9
    ```

=== "从右往左遍历"

    ``` mermaid
    graph LR
        S0["初始<br>─────<br>[ ]"]
        S1["① Push 73<br>─────<br>73"]
        S2["② Pop<br>─────<br>[ ]"]
        S3["③ Push 76<br>─────<br>76"]
        S4["④ Push 72<br>─────<br>72<br>76"]
        S5["⑤ Push 69<br>─────<br>69<br>72<br>76"]
        S6["⑥ Pop<br>─────<br>72<br>76"]
        S7["⑦ Push 71<br>─────<br>71<br>72<br>76"]
        S8["⑧ Pop<br>─────<br>72<br>76"]
        S9["⑨ Pop<br>─────<br>76"]
        S10["⑩ Push 75<br>─────<br>75<br>76"]
        S11["⑪ Push 74<br>─────<br>74<br>75<br>76"]
        S12["⑫ Push 73<br>─────<br>73<br>74<br>75<br>76"]

        style S0 fill:#e3f2fd
        style S1 fill:#c8e6c9
        style S2 fill:#ffcdd2
        style S3 fill:#c8e6c9
        style S4 fill:#c8e6c9
        style S5 fill:#c8e6c9
        style S6 fill:#fff9c4
        style S7 fill:#c8e6c9
        style S8 fill:#fff9c4
        style S9 fill:#fff9c4
        style S10 fill:#c8e6c9
        style S11 fill:#c8e6c9
        style S12 fill:#c8e6c9
    ```

更多例题：

- [商品折扣后的最终价格](https://leetcode.cn/problems/final-prices-with-a-special-discount-in-a-shop/description/)
- [下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/description/)
- [下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/description/)
- [股票价格跨度](https://leetcode.cn/problems/online-stock-span/description/)

=== "商品折扣后的最终价格"

    ``` cpp
    class Solution {
    public:
        vector<int> finalPrices(vector<int>& prices) {
            // 题意即找到首个小于等于栈顶值的下标
            auto ans = prices;
            stack<int> st;
            int n = prices.size();
            for (int i = 0; i < n; ++i) {
                while (!st.empty() && prices[i] <= prices[st.top()]) {
                    // 当前值小于等于栈顶元素
                    ans[st.top()] -= prices[i];
                    // 栈顶元素的答案已经得出
                    st.pop();
                }
                st.push(i);
            }
            return ans;
        }
    };
    ```

=== "下一个更大元素 I"

    ``` cpp
    class Solution {
    public:
        vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
            // 本质上，我们需要找到nums2中所有数字的下一个更大数字
            stack<int> st;
            unordered_map<int, int> next;
            for (int x : nums2) {
                while (!st.empty() && x > st.top()) {
                    next[st.top()] = x;
                    st.pop();
                }
                st.push(x);
            }

            int n = nums1.size();
            vector<int> ans(n, -1);
            for (int i = 0; i < n; ++i)
                if (next.contains(nums1[i]))
                    ans[i] = next[nums1[i]];
            return ans;
        }
    };
    ```

=== "下一个更大元素 II"

    ``` cpp
    class Solution {
    public:
        vector<int> nextGreaterElements(vector<int>& nums) {
            int n = nums.size();
            vector<int> ans(n, -1);
            stack<int> st;
            for (int i = 0; i < 2 * n; ++i) {
                while (!st.empty() && nums[i % n] > nums[st.top()]) {
                    ans[st.top()] = nums[i % n];
                    st.pop();
                }
                st.push(i % n);
            }
            return ans;
        }
    };
    ```

=== "股票价格跨度"

    ``` cpp
    class StockSpanner {
    public:
        StockSpanner() {
            st.emplace_back(INT_MAX, 0);
        }

        int next(int price) {
            while (!st.empty() && price >= st.back().first)
                st.pop_back();
            int ans = ++i - st.back().second;
            st.emplace_back(price, i);
            return ans;
        }

        int i = 0;
        vector<pair<int, int>> st;
    };
    ```
