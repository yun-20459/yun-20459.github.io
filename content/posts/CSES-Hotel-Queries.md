---
title: CSES Hotel Queries
date: 2022-09-17 23:13:51
tags: [CSES, segment tree]
---

## 題目

給一個序列 $a$，對於每個詢問 $x$ 找到最小的 $i$ 使得 $a[i] >= x$，並將 $a[i] -= x$。

## 作法

這題要用到的技巧是線段樹二分搜。

線段樹的每個節點可以存「節點包含的區間內最大的數」，所以對於每個詢問 $x$，我都可以去看是下面哪個 case

1. 左子樹的最大值 $>= x$ -> 往左子樹遞迴查詢
2. 右子樹的最大值 $>= x$ -> 往右子樹遞迴查詢
3. 都小於 -> 答案 $= 0$

實作就看 code

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;

const int mxn = 2e5 + 5;
int ans;

struct Seg {
  ll seg[mxn << 2], a[mxn];

  void pullup(int id) {
    seg[id] = max(seg[id << 1] , seg[id << 1 | 1]);
  }
  void build(int l, int r, int id = 1) {
    if (l == r) {
      seg[id] = a[l];
      return;
    }
    int mid = (l + r) >> 1;
    build(l, mid, id << 1);
    build(mid + 1, r, id << 1 | 1);
    pullup(id);
  }

  void bs(int l, int r, ll val, int id = 1) {
    // binary search on segment tree
    if (l == r) {
      if (seg[id] >= val) ans = l;
      seg[id] -= val;
      return;
    }
    int mid = (l + r) >> 1;
    if (seg[id << 1] >= val) bs(l, mid, val, id << 1);
    else if (seg[id << 1 | 1] >= val) bs(mid + 1, r, val, id << 1 | 1);
    pullup(id);
  }
} seg;

void solve()
{
   int n, q;
   cin >> n >> q;
   for (int i = 1; i <= n; i++) cin >> seg.a[i];
   seg.build(1, n);
   while (q--) {
    ll x; cin >> x;
    ans = 0;
    seg.bs(1, n, x);
    cout << ans << " ";
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
