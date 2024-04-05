---
title: Leetcode 473. Matchsticks to Square
date: 2022-07-12 23:20:53
tags: [Leetcode, dp, bit manipulation, dfs]
---

不會 dp 也不會 bit manipulation QAQ

## 題目

[link](https://leetcode.com/problems/matchsticks-to-square/)

## 作法

### dfs

一個最簡單的做法可以直接 recursion, code 如下

```cpp
class Solution {
public:
    bool chk(vector<int>& side, vector<int>& matches, int id, int n)
    {
        if (id == n)
        {
            return side[0] == side[1] and side[1] == side[2] and side[2] == side[3];
        }
        
        for (int i = 0; i < 4; i++)
        {
            side[i] += matches[id];
            if (chk(side, matches, id + 1, n)) return true;
            side[i] -= matches[id];
        }
        
        return false;
    }
    bool makesquare(vector<int>& matches) {
        int n = match.size();
        vector<int> sideLen(4, 0);
        return chk(sideLen, matches, id, n);
    }
};
```

但是這樣會 TLE，所以需要做一點優化

1. 如果總火柴長度 mod 4 $\neq$ 0 的話，就不可能拚成正方形
2. 根據 1.，如果已經有邊的長度 > 總火柴長度 / 4，就不用繼續遞迴
3. 如果前面有一樣的 sideLen 檢查過不可行，就不用繼續遞迴
4. 把火柴長度從大排到小，這樣就可以比較快 return false

```cpp
class Solution {
public:
    #define ll long long int
    bool chk(vector<ll>& side, vector<int>& matches, int id, int n, const ll target)
    {
        if (id == n)
        {
            return side[0] == side[1] and side[1] == side[2] and side[2] == side[3];
        }
        
        for (int i = 0; i < 4; i++)
        {
            if (side[i] + (ll)matches[id] > target) continue;
            int j = i;
            while (--j >= 0)
            {
                if (side[i] == side[j]) break;
            }
            if (j != -1) continue;
            side[i] += (ll)matches[id];
            if (chk(side, matches, id + 1, n, target)) return true;
            side[i] -= (ll)matches[id];
        }
        
        return false;
    }
    bool makesquare(vector<int>& matches) {
        int n = matches.size();
        vector<ll> sideLen(4, 0);
        ll sum = 0;
        for (int i = 0; i < n; i++)
        {
            sum += (ll)matches[i];
        }
        if (sum % 4) return false;
        ll target = sum / 4;
        sort(matches.begin(), matches.end(), greater<int>());
        return chk(sideLen, matches, 0, n, target);
    }
};
```

### bit manipulation + dp

~~上面都不是重點~~

下面才是我真的想要分享的 part，在 discussion 看到這個做法覺得太漂亮了 ><

首先還是先需要知道每個邊的長度，接下來就用 bit 來表示取了哪幾個火柴，如果有一個 subset 可以拼成一條正方形的邊的話，那麼我就把這個 subset 存起來，然後利用 and 去看前面存起來的有沒有跟我是沒有交集的 subset，如果有的話代表我可以形成兩條邊，剩下就是利用 xor 去檢查全部的 set 扣掉這兩條邊是不是可以形成剩下的兩條邊

```cpp
class Solution {
public:
    #define ll long long int
    bool makesquare(vector<int>& v) {
        ll sum = 0;
        int n = v.size();
        for (int i = 0; i < n; i++)
        {
            sum += (ll)v[i];
        }
        if (sum % 4) return false;
        
        ll sideLen = sum / 4;
        int allset = (1 << n) - 1;
        vector<bool> validHalf(1 << n, false);
        vector<int> availableMask;
        for (int mask = 0; mask <= allset; mask++)
        {
            ll subsetNum = 0;
            for (int i = 0; i < 32; i++)
            {
                subsetNum += ((mask >> i) & 1) ? (ll)v[i] : 0LL;
            }
            if (subsetNum == sideLen)
            {
                for (int am : availableMask)
                {
                    if ((am & mask) == 0)
                    {
                        int validhalf = am | mask;
                        validHalf[validhalf] = true;
                        if (validHalf[allset ^ validhalf]) return true;
                    }
                }
                availableMask.push_back(mask);
            }
        }
        
        return false;
    }
};
```
