---
title: CSES Josephus Queries
date: 2022-10-03 22:39:07
tags: [CSES, math]
---

## 題目

給 $n$ 個圍成一圈的小孩，每 2 個人就移掉 2 個人中的第二個，問第 $k$ 個小孩被移掉的編號

## 作法

這題網路上有很多題解，我看的懂 tutorial 但實作都看不懂所以寫一下。 ps 是我隊友教我的她好棒

因為圍成一圈，所以說我們可以把要移掉的數目看成是 $\frac{n + 1}{2}$，這樣一來如果 $n$ 是奇數，被移掉的就會是所有的偶數跟 1，反之則是移掉所有的偶數。

那麼遞迴下去做的話，可以看成是把該移掉的人移掉之後重新編號成 1, 2, 3, ...，再把 index remap 回來，所以這時候就要看原本是奇數還是偶數來決定怎麼 remap。

舉例來說，如果 $n$ 是奇數，那第一輪拿完會變成 3, 5, 7, ..., n，這樣重新編號成 1, 2, 3, ...，就要 $2 \times \text{id} + 1$ remap 回來。如果 $n$ 是偶數的話，那第一輪拿完會變成 1, 3, 5, 7, ..., n，這樣重新編號成 1, 2, 3, ...，就要 $2 \times \text{id} - 1$ remap 回來。

希望這樣舉例解釋會比較好懂 remap 的部分 QQ （我就是卡在這裡

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

int f(int n, int k) {
  if (n == 1) return 1;
  int kill = (n + 1) / 2;
  if (k <= kill) {
    if (2 * k > n) return 2 * k % n;
    return 2 * k;
  }
  int tmp = f(n >> 1, k - kill);
  if (n & 1) return 2 * tmp + 1;
  return 2 * tmp - 1;
}

void solve()
{
  int n, k; cin >> n >> k;
  cout << f(n, k) << endl;
}

int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int t = 1;
  cin >> t;
  while (t--)
  {
    solve(); 
  }
  return 0;
}
```
