---
title: Leetcode 1354. Construct Target Array With Multiple Sums
date: 2022-06-24 20:55:17
tags: [Leetcode, priority_queue]
---

## 題目

[link](https://leetcode.com/problems/construct-target-array-with-multiple-sums/)

## 作法

我是看 hint 才會的 QQ，hint 就是這題其實要反著做，就是說如果 target array 是可以被構造出來的話，逆推回去就應該是對的。

作法是把最大值一個一個從 priority_queue 中拔出來處理，有三種情況應該要 `return false`

1. `val <= sum`
    可以假設被修改前值是 $x$，剩下的值總和是 $y$，所以修改後的值是 $x+y$。 從 code 裡可以看到現在 $sum = y$，所以理論上是不會大於 $x + y$ 的。

2. `sum < 1`
    根據 1. 可以發現既然 $sum = y$，那因為要構造的陣列值都是 1 ，所以如果小於 1 就是錯的。

3. `sum != 1 and val == 0`
    這邊牽扯到我上面的一點實作方式，因為同一個值可能被操作很多次，所以可以直接 `%=sum` 找到操作前的值，再把它加上 $sum$ 就是修改前的陣列總和。那這邊需要特判`[1, 1]`這個 case，因為雖然理論上`val == 0`會是錯的（因為這代表修改前的值是 0 < 1），但是可以發現這個 case 在這裡會是錯的，所以需要特判掉。

```cpp
class Solution {
public:
    bool isPossible(vector<int>& target) {
        priority_queue<int> pq;
        unsigned int sum = 0;
        for (int num : target)
        {
            pq.push(num);
            sum += num;
        }
        int val = -1;
        while (pq.top() != 1)
        {
            val = pq.top();
            pq.pop();
            sum -= val;
            if (val <= sum || sum < 1) return false;
            val %= sum;
            sum += val;
            if (sum != 1 and val == 0) return false;
            pq.push(val);
        }
        
        return true;
    }
};
```
