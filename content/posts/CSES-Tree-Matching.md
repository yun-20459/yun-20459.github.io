---
title: CSES Tree Matching
date: 2022-09-19 21:51:48
tags: [CSES, dp, tree]
---

第一次接觸樹 dp OuO

## 題目

給一棵樹，找到最大的邊集合使得任兩邊不共享一個點，輸出集合大小。

## 作法

### 定義

如果是 array 版本的話（i.e. 給一個序列不能選相鄰的，最大化價值），比較容易可以想到轉移式 $dp(i) = max(v_i + dp(i - 2), dp(i - 1))$

轉成樹的版本的話，就需要在樹上面做 dp，對於一個節點 $u$ 有下面兩種情況

1. 取一條跟 $v \in child(u)$ 有連接的邊，答案定義為 $dp_0(u)$
2. 不取任何跟 $v \in child(u)$ 有連接的邊，答案定義為 $dp_1(u)$
  
為什麼沒有取兩條以上的 case 呢？因為如果取兩條以上的話 $u$ 就會變成那個共享的點。

所以從上面的定義可以知道答案是 $max(dp_0(1), dp_1(1))$

### 轉移式

- $dp_1(u)$

  因為不取任何跟 $v \in child(u)$ 有連接的邊，所以不管「跟 $v$ 的小孩有連接的邊」取不取都可以，所以答案就是

  $dp_1(u) = \sum_{v \in child(u)} max(dp_0(v), dp_1(v))$

- $dp_0(u)$

  可以先處理取一條的部分，因為取了 $u \rightarrow v$ 這一條，所以「不取任何跟 $w \in child(v)$ 有連接的邊」是 $dp_1(v)$，再加上 $u \rightarrow v$ 這一條，就是 $dp_1(v) + 1$。
  接下來要加上 $w \in child(u), w \neq v$ 的部分，畫一下圖就會發現就是

  $\sum_{w \in child(u), w \neq v} max(dp_0(w), dp_1(w))$

  那這個東西其實前面算過了，就是 $dp_1(u) - max(dp_0(v), dp_1(v))$

  所以綜合起來就是

  $dp_0(u) = dp_1(v) + 1 + dp_1(u) - max(dp_0(v), dp_1(v))$

### code

要注意 $dp_0(u)$ 會用到 $dp_1(u)$ 的結果，所以要先算 $dp_1(u)$

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;

const int mxn = 2e5 + 5;

int dp[mxn][2];
vector<int> g[mxn];

void dfs(int u, int p) {
  for (int v : g[u]) {
    if (v != p) {
      dfs(v, u);
      dp[u][1] += max(dp[v][0], dp[v][1]);
    }
  }
  for (int v : g[u]) {
    if (v != p) {
      dp[u][0] = max(dp[u][0], dp[v][1] + 1 + dp[u][1] - max(dp[v][0], dp[v][1]));
    }
  }
}

void solve()
{
#define eb emplace_back
  int n; cin >> n;
  for (int i = 0, x, y; i < n - 1; i++) {
    cin >> x >> y;
    g[x].eb(y);
    g[y].eb(x);
  }
  dfs(1, 0);
  cout << max(dp[1][0], dp[1][1]) << endl;
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
