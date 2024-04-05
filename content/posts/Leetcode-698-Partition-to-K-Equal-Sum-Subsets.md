---
title: Leetcode 698. Partition to K Equal Sum Subsets
date: 2022-07-13 18:17:01
tags: [Leetcode, dp, bit manipulation]
---

好像比較了解 bit manipulation 可以幹嘛了（？）

## 題目

[link](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)

## 做法

這題是 [這題](Leetcode-473-Matchsticks-to-Square.md) 的進化版（？），要分成 $k$ 堆，一樣可以用 bit + dp 做，然後這邊還有一個小技巧是直接用餘數，這樣就可以直接在最後判是不是等於 0，如果不用餘數的話可能就要寫個 if 或者用更複雜的方式去判現在這個 element 能不能被放進這個 subset

```cpp
class Solution {
public:
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum % k) return false;
        int target = sum / k;
        int n = nums.size();
        vector<int> dp(1 << n, -1);
        dp[0] = 0;
        sort(nums.begin(), nums.end());
        for (int mask = 0; mask < (1 << n); mask++)
        {
            if (dp[mask] == -1) continue;
            for (int j = 0; j < n; j++)
            {
                if (!(mask & (1 << j)) and dp[mask] + nums[j] <= target)
                {
                    dp[mask | (1 << j)] = (dp[mask] + nums[j]) % target;
                }
            }
        }
        return dp[(1 << n) - 1] == 0;
    }
};
```
