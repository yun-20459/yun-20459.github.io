---
title: Leetcode 629. K Inverse Pairs Array
date: 2022-07-17 21:27:49
tags: [Leetcode, dp, sliding window]
---

## 題目

[link](https://leetcode.com/problems/k-inverse-pairs-array/)

## 作法

### $O(nk^2)$

我以為會 TLE 然後剛剛測發現其實不會 QQ，可以定義 $dp[i][j]$ 是使用 1 ~ $i$ 的數字後，可以構成 $j$ 個 inversion pair 的數量，那答案就會是 $dp[n][k]$。

轉移式的話可以觀察，如果我現在已經放好 1~3 然後我要把 4 放進去這個序列裡面，我可以有下面這幾種放法

| 放法 | 造成的逆序數對 |
| --- | --- |
| 4xxx | 3 |
| x4xx | 2 |
| xx4x | 1 |
| xxx4 | 0 |

所以就可以發現這樣的轉移式

$dp[i][j] = dp[i - 1][j - k]$, $\forall k \in [0, i - 1]$

```cpp
class Solution {
public:
    int kInversePairs(int n, int k) {
        const int mod = 1e9 + 7;
        vector<vector<int> > dp(n + 1, vector<int>(k + 1, 1));
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= k; j++) {
                for (int k = 0; k <= min(K, N - 1); k++)  {
                    dp[i][j] = (dp[i][j] + dp[i - 1][j - k]) % mod;
                }  
            }      
        }

        return dp[n][k];
    }
};
```

### $O(nk)$

同樣的概念，只是剛剛的轉移式可以再優化

- $j < i$

    $$
    \begin{aligned}
        dp[i][0] &= dp[i - 1][0] \\\\
        dp[i][1] &= dp[i - 1][0] + dp[i - 1][1] = dp[i][0] + dp[i - 1][1] \\\\
        dp[i][2] &= dp[i - 1][0] + dp[i - 1][1] + dp[i - 1][2] = dp[i][1] + dp[i - 1][2] \\\\
        &\vdots \\\\
        dp[i][j] &= dp[i][j - 1] + dp[i - 1][j]
    \end{aligned}
    $$

- $j >= i$

    $$
    \begin{aligned}
        dp[i][j] &= dp[i - 1][j] + dp[i - 1][j - 1] + \cdots + dp[i - 1][j - i + 1] + (dp[i - 1][j - i] - dp[i - 1][j - i]) \\\\
        &= dp[i - 1][j] + (dp[i - 1][j - 1] + \cdots + dp[i - 1][j - i + 1] + dp[i - 1][j - i]) - dp[i - 1][j - i] \\\\
        &= dp[i - 1][j] + dp[i][j - 1] - dp[i - 1][j - i]
    \end{aligned}
    $$

```cpp
class Solution {
public:
    int kInversePairs(int n, int k) {
        const int mod = 1e9 + 7;
        
        vector<vector<int> > dp(n + 1, vector<int>(k + 1));
        
        dp[0][0] = 1;

        for (int i = 1 ; i <= n; i++) {
            dp[i][0] = 1;
            for (int j = 1; j <= k; j++) {
                dp[i][j] = (dp[i][j - 1] + dp[i - 1][j]) % mod;
                if (j >= i) dp[i][j] = (dp[i][j] - dp[i - 1][j - i] + mod) % mod;
            }
        }
        
        return dp[n][k];
    }
};
```

## 題外話

mathjax 在 version 3 沒有支援 `\\` 換行，會被 render 掉，所以要寫 `\\\\`
