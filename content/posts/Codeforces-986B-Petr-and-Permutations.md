---
title: Codeforces 986B. Petr and Permutations
date: 2022-07-22 13:54:14
tags: [Codeforces, math, graph, dfs]
---

## 題目

有一個陣列 $\{1, 2, ..., N\}, (N \leq 10^6)$，Petr 喜歡對序列執行 $3n$ 次任取兩個數字交換的操作，Um_nik 喜歡做 $7n + 1$ 次，給定一個操作後的陣列，求是誰操作的。

## 作法

把 index 跟陣列數值想成是圖中有連接的點的話，一開始未操作的陣列會有 $n$ 個自環，每一次操作會增加或減少一個環，由 $3n$ 跟 $7n + 1$ 的奇偶性不同可以判斷是誰操作的。

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long int ll;
typedef pair<int, int> pii;
typedef vector<int> vi;
typedef vector<bool> vb;
typedef vector<pii> vii;
typedef vector<vector<int> > vvi;

#define AC ios_base::sync_with_stdio(0), cin.tie(0);
#define rep(i, j, k) for (int i = j; i < k; i++)
#define all(v) (v).begin(), (v).end()
#define pb(x) push_back(x)
#define endl '\n'
#define F first
#define S second
#define inf 0x3f3f3f3f
#define ninf 0xc0c0c0c0
#define eps 1e-9
#define err(a) cerr << #a << ": " << a << endl

void dfs(vvi& adj, vb& vis, int i)
{
    vis[i] = 1;
    for (auto u : adj[i]) {
        if (!vis[u]) dfs(adj, vis, u);
    }
}

int main() {

    AC;

    int n; cin >> n;
    vvi adj(n + 1, vector<int>());
    rep(i, 1, n + 1) 
    {
        int x; cin >> x;
        adj[i].pb(x);
    }

    int cycle = 0;
    vb vis(n + 1);
    rep(i, 1, n + 1)
    {
        if (!vis[i]) {
            cycle++;
            dfs(adj, vis, i);
        }
    }

    if (!(cycle % 2)) puts("Petr";)
    else puts("Um_nik");

    return 0;
}
```

另一個實作方法，寫起來比較快，不用開 vis 陣列，概念是一樣的。

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long int ll;
typedef pair<int, int> pii;
typedef vector<int> vi;
typedef vector<bool> vb;
typedef vector<pii> vii;
typedef vector<vector<int> > vvi;

#define AC ios_base::sync_with_stdio(0), cin.tie(0);
#define rep(i, j, k) for (int i = j; i < k; i++)
#define all(v) (v).begin(), (v).end()
#define pb(x) push_back(x)
#define endl '\n'
#define F first
#define S second
#define inf 0x3f3f3f3f
#define ninf 0xc0c0c0c0
#define eps 1e-9
#define err(a) cerr << #a << ": " << a << endl

int main() {

    AC;

    int n; cin >> n;
    vi v(n);
    rep(i, 1, n + 1) 
    {
        cin >> v[i];
    }

    int res = 0;
    rep(i, 1, n + 1)
    {
        if (!v[i]) continue;
        res ^= 1;
        int x = i;
        while (x) 
        {
            int y = v[x];
            v[x] = 0;
            x = y;
        }
    }

    if (res & 1) puts("Um_nik");
    else puts("Petr");

    return 0;
}
```
