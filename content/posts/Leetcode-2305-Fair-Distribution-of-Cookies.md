---
title: Leetcode 2305. Fair Distribution of Cookies
date: 2022-07-14 17:07:07
tags: [Leetcode, dp, bit manipulation]
---

## 題目

[link](https://leetcode.com/problems/fair-distribution-of-cookies/)

## 作法

## bit manipulation + dp

這題跟 [這題](Leetcode-698-Partition-to-K-Equal-Sum-Subsets.md) 蠻像的，不過這次是要盡可能平均分然後輸出這樣分以後一個小孩最多可能拿到的餅乾數。

我一開始想跟 698 那題一樣做，不過這樣的話是沒辦法知道 max 的，因為只能用袋子分而不是直接拿餅乾分，所以要換一個方法。

定義 `dp[i][mask]` 為當人數有 $i$ 個的時候取的 subset 為 $mask$ 時，拿到最多餅乾的小孩的餅乾數量的最小值，就會有以下的轉移式

`dp[i][mask] = min(dp[i][mask], max(dp[i - 1][mask ^ submask], sum[submask]))`

意思是說在這群小孩裡面有個小孩拿了 submask 這樣的 subset，剩下的小孩拿其他的，看最大值會是多少，然後因為我最後要的是最小化最大值，所以取 min。

關於 enumerating submask of mask 可以看 [這篇](https://cp-algorithms.com/algebra/all-submasks.html#iterating-through-all-masks-with-their-submasks-complexity-o3n) 講解

```cpp
class Solution {
public:
    int distributeCookies(vector<int>& cookies, int k) {
        int n = cookies.size();
        vector<int> sum(1 << n, 0);
        for (int mask = 0; mask < (1 << n); mask++)
        {
            for (int i = 0; i < n; i++)
            {
                if ((mask >> i) & 1)
                {
                    sum[mask] += cookies[i];
                }
            }
        }
        
        vector<vector<int> > dp(k + 1, vector<int>(1 << n, INT_MAX));
        dp[0][0] = 0;
        for (int person = 1; person <= k; person++)
        {
            for (int mask = 0; mask < (1 << n); mask++)
            {
                for (int submask = mask; submask; submask = (submask - 1)&mask)
                {
                    dp[person][mask] = min(dp[person][mask], 
                                          max(sum[submask], dp[person - 1][mask ^ submask]));
                }
            }
        }
        
        return dp[k][(1 << n) - 1];
    }
};
```

## 剪枝 dfs

code by [蕭啟湘](https://www.linkedin.com/in/啟湘-蕭-b36662211/)

概念是不管給那個小孩都是一樣的，在分第 $n$ 個餅乾的時候，只分成「拿過餅乾的小孩總數 + 1」種 case，因為拿過餅乾的小孩就不一樣了，剩下「+1」的 case 是指分給還沒有拿過餅乾的小孩的情況，下面為了可以更好地理解這段話故意把 for loop 最後一個 case 拆出來寫，其實是可以合併的啦

```cpp
class Solution {
public:
    int ans = INT_MAX;

    int dfs(vector<int>& child, vector<int>& cookies, int cookieBagId, int numberOfPeopleHavingCookies, int childNum, int size){

        if(cookieBagId == size) 
        {
            return *max_element(child.begin() , child.end());
        }

        int cases = min(numberOfPeopleHavingCookies + 1, childNum);

        for (int i = 0 ; i < cases - 1; i++) 
        {
            child[i] += cookies[cookieBagId];
            ans = min(ans, 
                      dfs(child, cookies, cookieBagId + 1 , numberOfPeopleHavingCookies, childNum, size));
            child[i] -= cookies[cookieBagId];
        }

        child[cases - 1] += cookies[cookieBagId];
        ans = min(ans, 
                  dfs(child, cookies, cookieBagId + 1 , numberOfPeopleHavingCookies + 1, childNum, size));
        child[cases - 1] -= cookies[cookieBagId];

        return ans;
    }

    int distributeCookies(vector<int>& cookies, int k) {
        vector<int> child(k, 0);
        return dfs(child, cookies, 0, 0, k, cookies.size());
    }
};
```
