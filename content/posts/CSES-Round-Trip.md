---
title: CSES Round Trip
date: 2022-02-27 22:41:56
tags: [CSES, graph]
---

我一直以為我會找環欸，結果其實是我會找但要印出來就不會，我就笨 :p

## 題目

[link](https://cses.fi/problemset/task/1669/)
找圖中隨便一個環然後印路徑，如果沒有就輸出 ```IMPOSSIBLE```。

## 作法

就是 dfs 一下(?)，最近在試著改掉開全域的壞習慣 :p，主要是我想要怎麼記祖先想很久，覺得開 ```parent``` 陣列又太肥，後來想到當參數傳下去不就好了嗎，笨笨的 :p

```cpp
#include <bits/stdc++.h>

#define AC                   \
    ios::sync_with_stdio(0); \
    cin.tie(0);              \
    cout.tie(0);

using namespace std;

#define DEBUG

void dfs(int id, vector<vector<int>> &adj, vector<bool> &vis, vector<int> &path, int parent)
{
    vis[id] = true;
    path.push_back(id);
    for (int nei : adj[id])
    {
        if (nei == parent)
        {
            continue;
        }

        if (vis[nei])
        {
            path.push_back(nei);
            int sz = path.size(), pos = -1;
            for (int i = 0; i < sz - 1; i++)
            {
                if (path[i] == nei)
                {
                    pos = i;
                    break;
                }
            }
            cout << sz - pos << "\n";
            for (int i = pos; i < sz; i++)
            {
                cout << path[i] << " ";
            }
            exit(0);
        }

        dfs(nei, adj, vis, path, id);
    }
    path.pop_back();
}

int main()
{
    AC;
    int n, m;
    cin >> n >> m;
    vector<vector<int>> adj(n + 1, vector<int>());
    vector<bool> vis(n + 1, 0);
    for (int i = 0; i < m; i++)
    {
        int s, e;
        cin >> s >> e;
        adj[s].push_back(e);
        adj[e].push_back(s);
    }

    for (int i = 1; i <= n; i++)
    {
        if (!vis[i])
        {
            vector<int> path;
            dfs(i, adj, vis, path, 0);
        }
    }

    cout << "IMPOSSIBLE\n";

    return 0;
}
```
