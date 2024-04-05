---
title: CSES List Removals
date: 2022-09-18 19:18:46
tags: [CSES, BIT]
---

## 題目

給定一個長度為 $n$ 的序列 $a$，給 $n$ 個詢問，對於每個詢問 $pos$，輸出位置為 $pos$ 的值並從序列中刪除。

## 作法

這題的技巧要用到「BIT 上二分搜」，可以想像如果一個數字還沒有被刪除，我們就將它的 presentation 記為 $1$，反之則為 $0$，那麼查詢位置在 $pos$ 的元素等於就是查找前綴和為 $pos$，且 presentation = 1 的位置。

至於二分搜要怎麼實作呢，BIT 的節點是根據 2 的冪次劃分的，所以每次可以嘗試擴展 2 的冪次，看現在的前綴和是不是小於詢問，如果是的話代表可以擴展。等於說我們在找 $\sum_{i = 1}^x presentation[i] < pos$ 的 $x$，如果現在的和還比詢問小的話就可以擴展。所以一路這樣擴展下去之後，我們會找到最大的 $x$ 符合上式，那麼 $x + 1$ 就是答案。

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;

const int mxn = 2e5 + 5;

int n, a[mxn];

int bits(int x) {
  // floor(log2(x))
  return x == 0 ? 0 : 31 - __builtin_clz(x);
}

struct BIT {
  int bit[mxn];

  int lowbit(int x) {
    return x & (-x);
  }

  void update(int pos, int val) {
    for (int i = pos; i <= n; i += lowbit(i)) bit[i] += val;
  }

  int query(int x) {
    int p = 0;
    for (int i = 1 << bits(n); i; i >>= 1) {
      if (p + i <= n && bit[p + i] < x) {
        x -= bit[p += i];
      }
    }
    return p + 1;
  }
} bit;

void solve()
{
  cin >> n;
  for (int i = 1; i <= n; i++) {
    cin >> a[i];
    bit.update(i, 1);
  }
  int p;
  for (int i = 1; i <= n; i++) {
    cin >> p;
    int res = bit.query(p);
    cout << a[res] << " ";
    bit.update(res, -1);
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
