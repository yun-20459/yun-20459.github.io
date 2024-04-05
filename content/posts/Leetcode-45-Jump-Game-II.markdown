---
title: Leetcode 45. Jump Game II
date: 2022-02-04 17:26:23
tags: [Leetcode, dp]
---

寒假因為很無聊就來刷題，絕對不是因為我很廢 :p

## 題目

[link](https://leetcode.com/problems/jump-game-ii/)

## 作法

我是先想到 $O(n^2)$ 的做法，對於每個位置 $i$ 都去看前面的位置 $j$ 能不能走得更遠，如果可以的話就把 $i$ 的最小步數更新成走到 $j$ 的步數 +1（從 $j$ 到 $i$）

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n, INT_MAX);
        dp[0] = 0;
        for (int i = 1; i < n; i++)
        {
            for (int j = 0; j < i; j++)
            {
                if (j + nums[j] >= i and dp[j] != INT_MAX)
                {
                    dp[i] = min(dp[i], dp[j] + 1);
                }
            }
        }
        
        return dp[n - 1];
    }
};
```

然後這題其實可以 $O(n)$，概念是一樣的，每個位置都去看可以走到多遠，如果走到前一次算出來最遠的地方，就更新成現在算出來可以到達最遠的地方，在到達之前就一直更新，就不用每次都回去重複算到底怎麼走最快。

比較好的解釋方式可能是如果我可以從 A 點走到 B 點，那麼我當然可以從 A 點走到介在 A、B 點中間的 C 點，那麼如果 C 點可以讓我走到更遠，```jumps++```就可以想成是原本想要走到 B 點，改成走到 C 點再繼續走。

code 我好懶得打，借別人的（Ｘ

然後這邊 for loop 寫 `i < n - 1` 是因為實作的關係，這個實作方式如果寫 `i < n` 的話，`[2, 2, 1, 1, 4]` 這種 case 會多算一步，而且這種實作方式最後一個可以跳多遠根本不重要，因為他是要求到達最後一個 index。

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int jumps = 0, curr = 0, farthest = 0, n = nums.size();
        
        for (int i = 0; i < n - 1; i++)
        {
            farthest = max(farthest, i + nums[i]);
            if (i == curr)
            {
                curr = farthest, jumps++;
            }
        }
        
        return jumps;
    }
};
```

提供另一種實作方法，就是走過去才算跳的步數。

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int jumps = 0, curr = 0, farthest = 0, n = nums.size();
        
        for (int i = 0; i < n; i++)
        {
            if (i > curr) 
            {
                jumps++;
                curr = fathest;
            }
            farthest = max(farthest, i + nums[i]);
        }
        
        return jumps;
    }
};
```
