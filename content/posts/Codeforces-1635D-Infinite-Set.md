---
title: Codeforces 1635D. Infinite Set
date: 2022-10-24 21:57:08
tags: [Codeforces, bitmask, dp, number theory, string]
---

這題好難，我看了好多遍別人的題解才懂 QwQ

## 題目

給一個陣列，考慮一個無限的集合 $S$ 由符合以下條件之一的 $x$ 構成

1. $x = a[i]$
2. $x = 2 * y + 1$ and $y \in S$
3. $x = 4 * y$ and $y \in S$

問在集合 $S$ 中有多少元素小於 $2^p$，答案很大要 mod $1e9+7$

## 作法

因為 $p$ 的範圍會到 2e5，所以要用二進制來想這題。

對於一個二進位制為 $t$ 的數字，可以想像操作二就是左移一位然後末位補 1，操作二就是左移兩位（這邊就有點像 string）。所以可以觀察出一個結論，就是對於一個數字，把它增加 $i$ 位的方法有 $dp_i$ 種，這裡的 $dp$ 就是費波那契數列。

那這跟答案有什麼關係呢，假設一個二進位制為 $t$ 的數字存在在 $a$ 之中，那麼他擴展出的數字要小於 $p$ 的話，他擴展出的數字的數量就會是 $\sum_{i = 0}^{p - t} dp_i$，也就是不擴展、擴展一位、擴展兩位...，那這件事情可以透過在算費波那契數列的時候算前綴和得到。

接下來考慮是不是每個 $a[i]$ 都會對答案有貢獻，其實不是這樣，因為 $S$ 是一個 set，不會有重複的數字出現，所以如果會重複出現就不用算。那題解的做法是說那就把會產生的數字想成一棵樹，我一直去看產生 $x$ 的 parent 有沒有已經被 create 過，如果有的話，那就代表 $x$ 已經被前面的數字（也就是 parent） create 過了，那就不能算進答案裡面（因為已經算過了），在這邊的話 parent 的意思就是說看他能不能逆推操作二或操作三。

詳細實作就看 code，我一直想說為什麼大家都要用 map 記不用 set，寫起來才發現這樣 code 會好寫一點。

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
#define SZ(a) (int)(a).size()
#define endl '\n'
#define X first
#define Y second

const int mod = 1e9 + 7;
const int mxn = 2e5 + 5;
int a[mxn];
ll pre[mxn], dp[mxn];
ll ans;

void init() {
  dp[0] = dp[1] = 1;
  pre[0] = 1, pre[1] = 2;
  for (int i = 2; i < mxn; i++) {
    dp[i] = (dp[i - 1] + dp[i - 2]) % mod;
    pre[i] = (pre[i - 1] + dp[i]) % mod;
  }
}

void solve()
{
  int n, p; cin >> n >> p; 
  init();
  set<int> st;
  for (int i = 0; i < n; i++) {
    cin >> a[i];
    st.insert(a[i]);
  }
  
  for (int i = 0; i < n; i++) {
    int flag = 0, x = a[i];
    while (x) {
      if (x % 2) x >>= 1;
      else if (x % 4 == 0) x >>= 2;
      else break;
      if (st.find(x) != st.end()) {
        flag = 1;
        break;
      }
    }
    if (flag) continue;
    int len = __lg(a[i]) + 1;
    if (p - len >= 0) {
      ans = (ans + pre[p - len]) % mod;
    }
  }

  cout << ans << endl;
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
