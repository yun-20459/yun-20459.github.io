---
title: Leetcode 1696. Jump Game VI
date: 2022-07-10 00:19:10
tags: [Leetcode, dp, priority_queue, deque]
---

## 題目

[link](https://leetcode.com/problems/jump-game-vi/)

## 作法

令 `dp[i]` 為以 $i$ 為起點到終點能獲得的最大分數。如果枚舉可以走的範圍內最大的話複雜度會是 $O(nk)$ 會 TLE，所以可以用 deque 或是 priority_queue 來優化，維護一個可以走的範圍內的單調隊列或者 heap，就可以在 $O(1)$ 或是 $O(logk)$ 內拿到最大值了。

下面兩種 code 分別用 deque 和 priority_queue 寫， deque 版本是從起點開始做，priority_queue 版本是從終點開始做

deque 版本

```cpp
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> dp(n, 0);
        deque<int> dq;
        dp[0] = nums[0];
        dq.push_back(0);
        for (int i = 1; i < n; i++)
        {
            if (dq.front() < i - k) dq.pop_front();
            dp[i] += nums[i] + dp[dq.front()];
            
            while (!dq.empty() && dp[i] >= dp[dq.back()]) dq.pop_back();
            
            dq.push_back(i);
        }
        
        return dp[n - 1];
    }
};
```

priority_queue 版本

```cpp
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        priority_queue<pair<int, int> > pq;
        vector<int> dp(n, 0);
        
        for (int i = n - 1; i >= 0; i--)
        {
            while (!pq.empty() and pq.top().second > i + k) pq.pop();
            dp[i] = nums[i];
            dp[i] += (pq.empty() ? 0 : pq.top().first);
            pq.push({dp[i], i});
        }
        
        return dp[0];
    }
};
```
