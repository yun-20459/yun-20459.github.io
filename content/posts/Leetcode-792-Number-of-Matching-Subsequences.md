---
title: Leetcode 792. Number of Matching Subsequences
date: 2022-07-20 14:07:36
tags: [Leetcode, binary search, string]
---

試著記得每次都要寫複雜度（？

## 題目

[link](https://leetcode.com/problems/number-of-matching-subsequences/)

## 作法

一開始看到 tag 以為真的要寫 trie，後來才發現不用（用 trie 我也不會寫），用二分搜就可以了。

概念是先去紀錄 s 每個字母有出現在哪裡，然後去遍歷每個 word，對於 word 裡的每一個字母，我都要能在 s 裡面找到比前一個字母在 s 裡面的位置更後面的位置（好拗口），這樣才符合 subsequence of string。

舉例來說

```cpp
s = "abcd"
words = ["ab", "cb"]
```

顯然對於 `"ab"` 來說，我可以在 s 裡面找到 1 跟 2 這兩個位置，所以他是對的，但對於 `"cb"` 來說，c 的第一個位置就是 3 了，而 3 後面沒有 `b` 所以他是錯的。

假設 s 長度是 $L$，$words$ 陣列長度是 $N$，每個 word 長度是 $l$，複雜度會是 $O(LNlog(l))$。

```cpp
class Solution {
public:
    int numMatchingSubseq(string s, vector<string>& words) {
        vector<vector<int> > ch(26);
        int n = s.length();
        for (int i = 0; i < n; i++)
        {
            ch[s[i] - 'a'].push_back(i);
        }
        
        int ans = 0;
        for (auto &word: words)
        {
            int id = -1;
            bool found = true;
            for (char c : word)
            {
                auto idx = upper_bound(ch[c - 'a'].begin(), ch[c - 'a'].end(), id);
                if (idx == ch[c - 'a'].end()) {
                    found = false;
                    break;
                }
                id = *idx;
            }
            if (found) ans++;
        }
        
        return ans;
    }
};
```
