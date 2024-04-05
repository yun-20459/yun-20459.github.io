---
title: Leetcode 32. Longest Valid Parentheses
date: 2022-07-07 21:17:42
tags: [Leetcode, dp]
---

我真的不會 dp QwQ

## 題目

[link](https://leetcode.com/problems/longest-valid-parentheses/)

## 作法

可以先從基礎題開始想，就是判合法括號字串，這個用 stack 就可以解決（其實這題也可，但我想練 dp），從這個的想法出發的話，可以先想到

- 如果前一個是 `(` 後一個是 `)`，那就有一個長度為 2 的合法括號字串

如果以 `dp[i]` 來記錄以 $i 為結尾最長合法括號字串長度的話， 1. 可以寫成 `dp[i] = dp[i - 1] + 2`，要注意這個是建立在你已經知道前面有 `(` 了。

接下來再考慮這個 case `()()`，因為 `dp[i]` 要記錄的是以 $i$ 為結尾最長合法括號字串長度，所以前面合法括號字串長度也要算進來，這個的算法可以利用現在紀錄的長度跟 index 來算。

以這個 case 來說，它其實可以切成兩個 `()`，在算第二個的時候按照上面的式子只會記錄到長度 2，，所以要去看 `i - dp[i]` 這個 index dp 值存的是多少然後加起來。

就有

- `if (i - dp[i] > 0) dp[i] += dp[i - dp[i]]`

最後就是看整個字串然後一邊用 dp 值更新答案

```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.length(), left = 0, ans = 0;
        vector<int> dp(n, 0);
        for (int i = 0; i < n; i++)
        {
            if (s[i] == '(') {
                left++;
            }
            else if (left > 0) {
                dp[i] = dp[i - 1] + 2;
                dp[i] += (i > dp[i]) ? dp[i - dp[i]] : 0;
                ans = max(ans, dp[i]);
                left--;
            }
        }
        
        return ans;
    }
};
```
