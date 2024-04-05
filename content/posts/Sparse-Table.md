---
title: Sparse Table
date: 2022-09-04 22:13:25
tags: [CSES]
---

## 能幹嘛

區間最小/最大查詢，利用可以把區間拆成 $\log_2(\text{區間長度})$ 個長度為 2 的冪次的區間，達到 $O(N\log(N))$ pre-build, $O(1)$ 查詢。
（區間和查詢也可以，不過是 $O(\log(N))$，而且可以用前綴和 $O(1)$）

舉例來說，$[2, 10]$ 是一個長度為 9 的區間，那他就可以被拆成 $[2, 9] \cup [10, 10]$，如果我可以提前知道這兩個區間的答案，那我就知道組合起來的答案。

建 st 表的話可以看下面例題的 code， $st[i][j] :=$ 從 $i$ 開始長度為 $2^j$ 的區間的答案，那因為這樣的區間可以再拆成兩個長度為 $2^{j - 1}$ 的區間，所以可以用這兩個區間求答案。

btw 缺點是如果陣列值有改動的話就要重蓋 st 表，所以適用於不會改值的時候用。

## 例題

[CSES-Static Range Minimum Queries](https://cses.fi/problemset/task/1647/)

- 作法
  
  就是 sparse table 模板題，不過這題的話不用跟求 sum 一樣拆成好幾個不重疊的區間，因為求 min 的話就算拆成兩個重疊的也沒關係，所以就暴力拆成兩個 $\log(r - l + 1)$ 長度的區間就可以查詢了。

- code

    ```cpp
    #include <bits/stdc++.h>

    using namespace std;

    typedef long long ll;
    typedef pair<int, int> pii;
    typedef pair<ll, ll> pll;

    const int mxn = 2e5 + 5;
    const int logn = 19;

    int st[mxn][logn]; // sparse table, store answer for [i, i + 2^j - 1]
    int a[mxn]; // array
    int lg[mxn]; // log value
    int n, q;

    void init()
    {
        lg[1] = 0;
        for (int i = 2; i < mxn; i++) lg[i] = lg[i / 2] + 1;
        for (int i = 0; i < n; i++) cin >> st[i][0];
        for (int j = 1; j < logn; j++)
        {
            for (int i = 0; i + (1 << j) <= n; i++)
            {
                st[i][j] = min(st[i][j - 1], st[i + (1 << (j - 1))][j - 1]);
            }
        }
    }

    void solve()
    {
        cin >> n >> q;
        init();

        while (q--)
        {
            int l, r;
            cin >> l >> r;
            l--, r--;
            int j = lg[r - l + 1];
            cout << min(st[l][j], st[r - (1 << j) + 1][j]) << endl;
        }
    }

    int main() {
        ios::sync_with_stdio(0);cin.tie(0);
        int t = 1;
        // cin >> t;
        while (t--)
        {
            solve(); 
        }
        return 0;
    }
    ```

## 參考資料

[cp algorithms](https://cp-algorithms.com/data_structures/sparse-table.html#range-sum-queries)
