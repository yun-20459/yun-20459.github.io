---
title: Atcoder Beginner Contest 315 題解
date: 2023-08-20 21:42:01
tags: [Atcoder]
---

## A - tcdr

直接跳過 aeiou 輸出就好

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
#define SZ(a) (int)(a).size()
#define ALL(v) (v).begin(), (v).end()
#define X first
#define Y second

#define dbg(x) cerr << #x << " = " << x << endl;

void solve() {
  string s; cin >> s;
  for (int i = 0; s[i]; i++) {
    if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u') continue;
      else cout << s[i];
  }
  cout << "\n"; 
}

/*
CheckList:
1. long long
2. mod
3. template
4. input/output format/content
5. test sample input/output
*/
int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int T = 1;
  // cin >> T;
  while (T--) solve();
  return 0;
}
```

## B - The Middle Day

算出總和之後 / 2，然後一個一個月慢慢算現在有沒有超過就好

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
#define SZ(a) (int)(a).size()
#define ALL(v) (v).begin(), (v).end()
#define X first
#define Y second

#define dbg(x) cerr << #x << " = " << x << endl;

void solve() {
  int m; cin >> m;
  vector<int> d(m);
  int sum = 0;
  for (int &x : d) {
    cin >> x;
    sum += x;
  }
  sum /= 2;
  for (int i = 0; i < m; i++) {
    if (sum >= d[i]) sum -= d[i];
    else {
      cout << i + 1 << " " << sum + 1 << "\n";
      return;
    }
  }
}

/*
CheckList:
1. long long
2. mod
3. template
4. input/output format/content
5. test sample input/output
*/
int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int T = 1;
  // cin >> T;
  while (T--) solve();
  return 0;
}
```

## C - Flavors

直接分兩個 case 算，如果最大的兩個不同 flavor 的話，那答案一定是他們相加，不然就去把所有 flavor 的最大拿出來，然後拿兩個最大的相加就好。

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
#define SZ(a) (int)(a).size()
#define ALL(v) (v).begin(), (v).end()
#define X first
#define Y second

#define dbg(x) cerr << #x << " = " << x << endl;

void solve() {
  int n; cin >> n;
  vector<pii> v(n);  
  for (auto &p : v) cin >> p.Y >> p.X;
  sort(ALL(v));
  ll res = 0;
  if (v[n - 1].Y == v[n - 2].Y) {
    res = v[n - 1].X + v[n - 2].X / 2;
    vector<int> mxf(n + 1, -1);
    for (int i = 0; i < n; i++) {
      mxf[v[i].Y] = max(mxf[v[i].Y], v[i].X);
    }
    ll mx = v[n - 1].X, second_mx = -1;
    for (int i = 1; i <= n; i++) {
      if (mxf[i] > second_mx && mxf[i] < mx) second_mx = mxf[i];
    }
    res = max(res, second_mx + mx);
  } else {
    res = v[n - 1].X + v[n - 2].X;
  }
  cout << res << "\n";
}

/*
CheckList:
1. long long
2. mod
3. template
4. input/output format/content
5. test sample input/output
*/
int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int T = 1;
  // cin >> T;
  while (T--) solve();
  return 0;
}
```

## D - Magical Cookies

死亡實作 @@，可以想像像題目那樣枚舉的話會是 $O(H^2W)$ 會 TLE，所以可以利用字元集很少這個特性來做，簡單來說就是先算好每行/列的每個字元有多少，然後每次就去看現在還沒被刪掉的有沒有字元是會整行/列都一樣的，就把他移掉，然後這個過程因為只有 $H + W$ 行+列，所以只會做到 $H + W$ 次。

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
#define SZ(a) (int)(a).size()
#define ALL(v) (v).begin(), (v).end()
#define X first
#define Y second

#define dbg(x) cerr << #x << " = " << x << endl;

void solve() {
  int h, w; cin >> h >> w;
  vector<string> v(h);
  for (auto &s : v) cin >> s;
  vector<vector<int>> cnth(26, vector<int>(h, 0)), cntw(26, vector<int>(w, 0));
  for (int i = 0; i < h; i++) {
    for (int j = 0; j < w; j++) {
      int ind = v[i][j] - 'a';
      cnth[ind][i]++;
      cntw[ind][j]++;
    }
  }
  int resh = h, resw = w;
  vector<bool> skiph(h, false), skipw(w, false);
  for (int _ = 0; _ < h + w; _++) {
    vector<pii> ch, cw;
    for (int i = 0; i < h; i++) {
      if (skiph[i]) continue;
      for (int j = 0; j < 26; j++) {
        if (cnth[j][i] == resw && resw >= 2) {
          ch.push_back({i, j});
        }
      }
    }
    for (int i = 0; i < w; i++) {
      if (skipw[i]) continue;
      for (int j = 0; j < 26; j++) {
        if (cntw[j][i] == resh && resh >= 2) {
          cw.push_back({i, j});
        }
      }
    }
    for (auto p : ch) {
      skiph[p.X] = true;
      for (int i = 0; i < w; i++) cntw[p.Y][i]--;
      resh--;
    }
    for (auto p : cw) {
      skipw[p.X] = true;
      for (int i = 0; i < h; i++) cnth[p.Y][i]--;
      resw--;
    }


  }
  cout << resh * resw << "\n";
}

/*
CheckList:
1. long long
2. mod
3. template
4. input/output format/content
5. test sample input/output
*/
int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int T = 1;
  // cin >> T;
  while (T--) solve();
  return 0;
}
```

## E - Prerequisites

乍看之下是簡單的拓墣排序，但是注意到他要求最小數量，所以有些根本不會在 DAG 裡的孤點我們就不管它，這時候就可以利用題目要求的是 read book 1，用一個 priority_queue 來代替平常拓墣排序用的 queue，這樣編號比較大的孤點在處理到有關 1 之前的 DAG 之前就會被先 pop 掉。

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
#define SZ(a) (int)(a).size()
#define ALL(v) (v).begin(), (v).end()
#define X first
#define Y second

#define dbg(x) cerr << #x << " = " << x << endl;

const int N = 2e5 + 5;
vector<int> g[N];
int deg[N];

void solve() {
  int n; cin >> n;
  for (int i = 1; i <= n; i++) {
    int c; cin >> c;
    for (int j = 0; j < c; j++) {
      int x; cin >> x;
      g[i].push_back(x);
      deg[x]++;
    }
  }
  priority_queue<int> q;
  for (int i = 1; i <= n; i++) {
    if (deg[i] == 0) q.push(i);
  }
  vector<int> res;
  while (!q.empty()) {
    int u = q.top(); q.pop();
    res.push_back(u);
    for (int v : g[u]) {
      --deg[v];
      if (deg[v] == 0) q.push(v);
    }
  }
  for (int i = SZ(res) - 1; i >= 0; i--) {
    if (res[i] == 1) break;
    else cout << res[i] << " ";
  }
}

/*
CheckList:
1. long long
2. mod
3. template
4. input/output format/content
5. test sample input/output
*/
int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int T = 1;
  // cin >> T;
  while (T--) solve();
  return 0;
}
```

## F - Shortcuts

待補。
