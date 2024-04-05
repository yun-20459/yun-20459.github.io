---
title: Atcoder Beginner Contest 313 題解
date: 2023-08-16 19:59:22
tags: [Atcoder]
mathjax: true
---

## A - To be Saikyo

直接找最大的是多少就好，注意要特判一下如果第一個就是最大的且沒有人跟他一樣大就不用加，不然還是要 +1。

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
#define pb emplace_back

#define dbg(x) cout << (#x " = ") << x << "\n";

void solve() {
  int n; cin >> n;
  vector<int> p(n);
  for (int &x : p) cin >> x;
  int mx = *max_element(ALL(p));
  if (mx == p[0]) {
    for (int i = 1; i < n; i++) {
      if (p[i] == mx) {
        cout << 1 << "\n";
        return;
      }
    }
    cout << "0\n";
  }
  else cout << mx + 1 - p[0] << "\n";
}

int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int T = 1;
  // cin >> T;
  while (T--) solve();
  return 0;
}
```

## B - Who is Saikyo?

這可以畫成一個有向圖，然後只有當入度為 0 的點只有一個的時候才判定得出最強的人。

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
#define pb emplace_back

#define dbg(x) cout << (#x " = ") << x << "\n";

const int N = 50 + 5;
int deg[N];

void solve() {
  int n, m; cin >> n >> m;
  for (int i = 0; i < m; i++) {
    int a, b; cin >> a >> b;
    deg[b]++;
  }
  vector<int> res;
  for (int i = 1; i <= n; i++) {
    if (deg[i] == 0) res.push_back(i);
  }
  if (SZ(res) == 1) cout << res[0] << "\n";
  else cout << "-1\n";
}

int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int T = 1;
  // cin >> T;
  while (T--) solve();
  return 0;
}
```

## C - Approximate Equalization 2

首先把 a 先排序一下一定不虧。最後的結果因為一定只能差 1，那麼平均分配給大家一定是最棒的，餘數就可以分給本來比較多的人，這樣操作次數就可以比較少。

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

#define dbg(x) cout << (#x " = ") << x << "\n";

void solve() {
  int n; cin >> n;
  vector<int> a(n);
  ll sum = 0;
  for (int &x : a) {
    cin >> x;
    sum += x;
  }
  sort(ALL(a));
  vector<int> b(n, sum / n);
  int r = sum % n;
  for (int i = n - 1; i > n - 1 - r; i--) b[i]++;
  ll res = 0;
  for (int i = 0; i < n; i++) res += abs(a[i] - b[i]);
  cout << res / 2 << "\n";
}

int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int T = 1;
  // cin >> T;
  while (T--) solve();
  return 0;
}
```

## D - Odd or Even

首先可以先把前 $k + 1$ 個弄成 $k$ 個式子，從這 $k$ 個式子中我們就可以先知道前 $k + 1$ 個數字的總和。例如說 $k = 3$ 的話就可以先問出下面這 3 條式子

$$
\begin{align\*}
A_1 + A_2 + A_3 &= s_1 \\\\
A_1 + A_2 + A_4 &= s_2 \\\\
A_1 + A_3 + A_4 &= s_3 \\\\
A_2 + A_3 + A_4 &= s_4 \\\\
\end{align\*}
$$

因為注意到這些相加之後除以 $k + 1$ 在減掉每一條就可以得到 $A_1 ~ A_4$（這邊實作上用 xor，因為 xor 有很好的性質是 a xor a = 0），那麼剩下的要怎麼問到呢，既然我們都有了 $A_1 + A_2 + ... + A_{k - 1}$，那我們就可以帶著這些東西再一次配一個新東西去問就可以了。

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

#define dbg(x) cout << (#x " = ") << x << "\n";

void out(vector<int> &v) {
  for (int x : v) cout << x << " ";
  cout << endl;
}

void solve() {
  int n, k; cin >> n >> k;
  vector<int> res(n);
  auto ask = [&](vector<int> v) {
    for (int &x : v) x++;
    cout << "? ", out(v);
    int x; cin >> x;
    return x;
  };
  int r = 0;
  for (int i = 0; i < k + 1; i++) {
    vector<int> v;
    for (int j = 0; j < k + 1; j++) {
      if (i != j) v.push_back(j);
    }
    res[i] = ask(v);
    r ^= res[i];
  }
  for (int i = 0; i < k + 1; i++) res[i] ^= r;
  vector<int> v(k);
  int s = 0;
  for (int i = 0; i < k - 1; i++) v[i] = i, s ^= res[i];
  for (int i = k + 1; i < n; i++) {
    v.back() = i;
    res[i] = s ^ (ask(v));
  }
  cout << "! ", out(res);
}

int main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int T = 1;
  // cin >> T;
  while (T--) solve();
  return 0;
}
```

## E - Duplicate

首先可以觀察到

### 觀察 1

如果有兩個相鄰的數字都 $\geq 2$ 那麼就會沒完沒了，然後剩下的都有解。

接下來

### 觀察 2

只有一個方法可以讓數字遞減，那就是只由一個數字構成的後綴，這個東西會慢慢地不見 ex. 11111 當後綴之類的。

### 觀察 3

如果有一串連續的 1 後面跟一個 char c，那麽這串東西等等就會變成遠本的 1 多加 $c - '1'$ 個 1。

有這樣就可以做了，官解超級聰明直接用了一個 encode 方法，就是把連續的 char，是誰有幾個記錄下來，這樣從後面算回來就快很多（畢竟這題的觀察都是有關 suffix）

encode 方法 example:

`223444445 -> {{2, 2}, {3, 1}, {4, 5}, {5, 1}}`

從後面算回來的方式是，如果當前的字元是 1，[代表剛剛算過的東西會根據上一個字元 expand](#觀察-3)，所以就把當前的那個字元數有多少加上前面算過的東西 * 需要的次數，那如果當前的字元不是 1 的話，代表他等等才要被 expand，就先加 1，加 1 是因為我要去累計有多少人的等等要被 expand，可以模擬一下就會發現這樣加乘的過程中就是在累計 expand 數量。

（我覺得我沒有解釋得很好 QQ）

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

const int M = 998244353;

vector<pair<char, ll>> encode(string &s) {
  if (SZ(s) == 0) return {};
  char c = s[0];
  int cnt = 1;
  vector<pair<char, ll>> v;
  for (int i = 1; s[i]; i++) {
    if (s[i] == c) cnt++;
    else {
      v.push_back({c, cnt * 1ll});
      c = s[i];
      cnt = 1;
    }
  }
  v.push_back({c, cnt});
  return v;
}

ll add(ll a, ll b) {
  return (a + b) % M;
}

void solve() {
  int n; string s; cin >> n >> s;
  for (int i = 0; i < n - 1; i++) {
    if (s[i] >= '2' && s[i + 1] >= '2') {
      cout << "-1\n";
      return;
    }
  }
  vector<pair<char, ll>> v = encode(s);
  ll res = 0;
  char last = '1';
  while (!v.empty()) {
    auto [c, cnt] = v.back();
    v.pop_back();
    if (c == '1') {
      cnt = add(cnt, res * (last - '1'));
      res = add(res, cnt);
    } else {
      res = add(res, 1);
    }
    last = c;
  }
  cout << (res - 1 + M) % M;
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
