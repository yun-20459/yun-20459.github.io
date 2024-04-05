---
title: Atcoder Beginner Contest 312 題解
date: 2023-08-17 21:57:46
tags: [Atcoder]
mathjax: true
---

## A - Chord

判斷一下就好了。

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

string ss[7] = {"ACE", "BDF", "CEG", "DFA", "EGB", "FAC", "GBD"};

void solve() {
  string s; cin >> s;
  for (int i = 0; i < 7; i++) {
    if (s == ss[i]) {
      cout << "Yes\n";
      return;
    }
  }
  cout << "No\n";
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

## B - TaK Code

可以直接手打出那個 pattern 然後比對一下就 ok

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

string pat[9] = {
  "###.?????",
  "###.?????",
  "###.?????",
  "....?????",
  "?????????",
  "?????....",
  "?????.###",
  "?????.###",
  "?????.###" };

void solve() {
  int n, m; cin >> n >> m;
  vector<string> g(n);
  for (auto &s : g) cin >> s;
  for (int i = 0; i <= n - 9; i++) {
    for (int j = 0; j <= m - 9; j++) {
      bool ok = true;
      for (int dx = 0; dx < 9 && ok; dx++) {
        for (int dy = 0; dy < 9 && ok; dy++) {
          if (pat[dx][dy] != '?' && g[i + dx][j + dy] != pat[dx][dy]) {
            ok = false;
          }
        }
      }
      if (ok) cout << i + 1 << " " << j + 1 << "\n";
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

## C - Invisible Hand

首先對 seller 跟 buyer 的錢 sort 不虧，然後可以發現這個錢是有單調性的，如果 x 元可以滿足的話那 x + 1 元一定可以，因為 seller 只會等於會變多，buyer 只會等於或變少，這樣 seller 一定 >= buyer。所以二分搜一下就好了。

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
  int n, m; cin >> n >> m;
  vector<int> seller(n), buyer(m);
  for (int &x : seller) cin >> x;
  for (int &x : buyer) cin >> x;
  sort(ALL(seller));
  sort(ALL(buyer));
  int l = 0, r = 1e9 + 1;
  auto chk = [&](int x) {
    int ind1 = upper_bound(ALL(seller), x) - seller.begin();
    int ind2 = buyer.end() - lower_bound(ALL(buyer), x);
    return ind1 >= ind2;
  };
  while (r - l > 1) {
    int m = (l + r) >> 1;
    if (chk(m)) r = m;
    else l = m;
  }
  cout << r << "\n";
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

## D - Count Bracket Sequences

看到 $2^x$ 又要 mod 998244353 就會先想到應該要 dp，然後合法匹配括號的話需要兩個條件

1. 左括號數量 = 右括號數量
2. 任何前綴的左括號數量 >= 右括號數量

所以努力想一下就會有下面的 dp 式，$dp_{i, j} = $ 前 $i$ 個字元裡，左括號 - 右括號的數量 = $j$

遞推式就像下面這樣

$$
dp_{i, j} =
  \begin{cases}
    dp_{i - 1, j - 1}   & \quad \text{if } s[i] = ( \\\\
    dp_{i - 1, j + 1}   & \quad \text{if } s[i] = ) \\\\
    dp_{i - 1, j - 1} + dp_{i - 1, j + 1} & \quad \text{if } s[i] = ?
  \end{cases}
$$

base case: $dp_{0, 0} = 1$，求 $dp_{n, 0}$

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

const int N = 3e3 + 5;
const int M = 998244353;
ll dp[N][N];

ll add(ll a, ll b) {
  return (a + b) % M;
}

void solve() {
  string s; cin >> s;
  int n = SZ(s);
  s = "@" + s;
  dp[0][0] = 1;
  for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= i; j++) {
      if (j == 0) {
        if (s[i] == '(') continue;
        dp[i][j] = add(dp[i][j], dp[i - 1][j + 1]);
      } else {
        if (s[i] == '(') dp[i][j] = add(dp[i][j], dp[i - 1][j - 1]);
        else if (s[i] == ')') dp[i][j] = add(dp[i][j], dp[i - 1][j + 1]);
        else dp[i][j] = add(dp[i][j], add(dp[i - 1][j - 1], dp[i - 1][j + 1]));
      }
    }
  }

  cout << dp[n][0] << "\n";
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

## E - Tangency of Cuboids

待補。

## F - Cans and Openers

首先對於所有的東西都 sort 一下不虧，畢竟都是要收益最大。再來可以再把三個東西歸成兩類，第一類是自己就可以開，第二類是互相依賴的關係，那最後我希望找到所有組合第一類取 $i$ 個，第二類取 $m - i$ 個，這樣我就可以 $O(m)$ 找出最大的那種組合。

第一類因為可以自己開，所以 sort 之後前綴和就好。

第二類的話可以用雙指針去慢慢爬，如果開罐器沒了就拿，如果開罐器還有就繼續開。

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
  int n, m; cin >> n >> m;
  vector<ll> a, b, c; 
  for (int i = 0; i < n; i++) {
    int t; ll x; cin >> t >> x;
    if (t == 0) a.push_back(x);
    else if (t == 1) b.push_back(x);
    else c.push_back(x);
  }
  sort(ALL(a), greater<>());
  sort(ALL(b), greater<>());
  sort(ALL(c), greater<>());
  vector<ll> x(m + 1, 0), y(m + 1, 0);
  for (int i = 1; i <= m; i++) {
    if (i - 1 < SZ(a)) x[i] = x[i - 1] + a[i - 1];
    else x[i] = x[i - 1];
  }
  int j = 0, use = 0, k = 0;
  for (int i = 1; i <= m; i++) {
    if (!use) {
      y[i] = y[i - 1];
      use = (j == SZ(c) ? 0 : c[j++]);
    } else {
      y[i] = y[i - 1] + (k == SZ(b) ? 0 : b[k++]);
      use--;
    }
  }
  ll res = 0;
  for (int i = 0; i <= m; i++) res = max(res, x[i] + y[m - i]);
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

## G - Avoid Straight Line

待補。
