---
title: Leetcode 96. Unique Binary Search Trees
date: 2022-02-12 19:13:27
tags: [Leetcode, dp, math]
---

忘記數學了所以來複習一下 :p

## 題目

[link](https://leetcode.com/problems/unique-binary-search-trees/)

## 作法

如果假設有 $n$ 個節點的二元樹有 $G_n$ 個，其中以 $i$ 節點為根的數目有 $F_i$ 個，那麼
$$ G_n = F_1 + F_2 + ... + F_n$$

根據二元樹的定義，根節點一定大於左節點小於右節點，所以以 $i$ 節點為根的二元樹可以看成是 **由 $1$ 到 $i - 1$ 構成的左子樹** 加上 **由 $i + 1$ 到 $n$ 構成的右子樹**，也就是說
$$F_i = G_{i - 1} * G_{n - i}$$

所以遞迴式就跑出來了

$$G_n = G_0 * G_{n - 1} + G_1 * G_{n - 2} + ... + G_{n - 1} * G_0$$

然後這個東西就是酷酷的卡塔蘭數列~ 因為有公式所以可以預處理然後 $O(1)$，但我還是喜歡這樣寫~

btw 公式是 $C_n = \frac{1}{n + 1} C^{2n}_n$，期中後面的 C 就是高中排列組合學的那個 C

```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        dp[1] = 1;
        for (int i = 2; i <= n; i++)
        {
            for (int j = 0; j < i; j++)
            {
                dp[i] += (dp[j] * dp[i - j - 1]);
            }
        }
        
        return dp[n];
    }
};
```
