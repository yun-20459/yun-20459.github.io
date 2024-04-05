---
title: Atcoder DP contest (持續更新中)
date: 2022-08-28 19:12:56
tags: [Atcoder, dp]
---

紀錄一下自己練習 dp 的過程，最近在寫 icpc 培訓班的作業感受到自己的 dp 實在是太爛了... 剛好被推薦有這個題單可以練習，所以就順便把自己不會的東西整理一下變成一篇文，希望可以加深一點印象w

btw 如果是我本來就會的東西就不會寫解釋純貼 code。

[題單](https://atcoder.jp/contests/dp/tasks)

## A. Frog 1

```cpp
const int maxn = 1e5 + 5;
int h[maxn], dp[maxn];

void solve()
{
    int n; cin >> n;
    for (int i = 1; i <= n; i++) cin >> h[i];
    dp[1] = 0, dp[2] = abs(h[2] - h[1]);
    for (int i = 3; i <= n; i++)
    {   
        dp[i] = min(dp[i - 1] + abs(h[i] - h[i - 1]), dp[i - 2] + abs(h[i] - h[i - 2]));
    }
    cout << dp[n] << endl;
}
```

## B. Frog 2

```cpp
const int maxn = 1e5 + 10;
int h[maxn], dp[maxn];

void solve()
{
    int n, k; cin >> n >> k;
    for (int i = 1; i <= n; i++) cin >> h[i];
    memset(dp, 0x3f, sizeof(dp));
    dp[1] = 0;
    for (int i = 2; i <= n; i++)
    {   
        for (int j = i - 1; j >= max(i - k, 1); j--)
        {   
            dp[i] = min(dp[i], dp[j] + abs(h[i] - h[j]));
        }
    }
    cout << dp[n] << endl;
}
```

## C. Vacation

```cpp
const int maxn = 1e5 + 5;
int val[3][maxn];
int dp[3][maxn];

void solve()
{
    int n; cin >> n;
    for (int i = 1; i <= n; i++) for (int j = 0; j < 3; j++) cin >> val[j][i];
    for (int i = 0; i < 3; i++)
    {   
        dp[i][1] = val[i][1];
    }
    for (int i = 2; i <= n; i++)
    {   
        dp[0][i] = max(dp[1][i - 1], dp[2][i - 1]) + val[0][i];
        dp[1][i] = max(dp[0][i - 1], dp[2][i - 1]) + val[1][i];
        dp[2][i] = max(dp[0][i - 1], dp[1][i - 1]) + val[2][i];
    }
    cout << max({dp[0][n], dp[1][n], dp[2][n]}) << endl;
    
}
```

## D. Knapsack 1

```cpp
const int maxn = 100 + 5;
const int maxw = 1e5 + 5;
ll w[maxn], v[maxn];
ll dp[maxn][maxw];

void solve()
{
    int n, W;
    cin >> n >> W;
    for (int i = 1; i <= n; i++) cin >> w[i] >> v[i];
    for (int i = 1; i <= n; i++)
    {   
        for (int j = 1; j <= W; j++)
        {   
            if (j >= w[i]) dp[i][j] = max(dp[i - 1][j - w[i]] + v[i], dp[i - 1][j]);
            else dp[i][j] = dp[i - 1][j];
        }
    }
    cout << dp[n][W] << endl;
}
```

## E. Knapsack 2

因為這次的重量上限到 1e9，所以不能直接套 D 的做法，但這次的價值上限是 1e3，所以可以利用這點，把狀態定成 $dp_{i, j} := $ 考慮第 $i$ 個物品的時候，價錢拿到 $j$ 的重量最小值。

```cpp
const int maxv = 1000 + 5;
const int maxn = 100 + 5;
int dp[maxn][maxn * maxv];
int w[maxn], v[maxn];

void solve()
{
    int N, W; cin >> N >> W;
    for (int i = 1; i <= N; i++) cin >> w[i] >> v[i];
    memset(dp, 0x3f, sizeof(dp));
    dp[0][0] = 0;
    for (int i = 1; i <= N; i++)
    {   
        for (int j = 0; j <= N * 1000; j++)
        {   
            if (j < v[i]) dp[i][j] = dp[i - 1][j];
            else dp[i][j] = min(dp[i - 1][j], dp[i - 1][j - v[i]] + w[i]);
        }
    }
    int ans;
    for (int j = 0; j <= N * 1000; j++)
    {   
        if (dp[N][j] <= W) ans = j;
    }
    cout << ans << endl;
}
```

## F. LCS

這題的重點是要會輸出一組解，那找的方式其實就是看當初怎麼算出來的，就反著回去做。

```cpp
const int maxn = 3000 + 5;
int dp[maxn][maxn];

void solve()
{
    string s, t;
    cin >> s >> t;
    int n1 = s.length(), n2 = t.length();
    for (int i = 1; i <= n1; i++)
    {   
        for (int j = 1; j <= n2; j++)
        {   
            if (s[i - 1] == t[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
            else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    // trace(dp[n1][n2]);
    string ans = "";
    int i = n1, j = n2;
    while (dp[i][j] > 0)
    {   
        if (s[i - 1] == t[j - 1]) {
            ans = s[i - 1] + ans;
            i--, j--;
        } else {    
            if (dp[i][j] == dp[i - 1][j]) i--;
            else j--;
        }
    }
    cout << ans << endl;
}
```

## G. Longest Path

有向無環圖找最長路，這題應該有蠻多作法的，不過我第一個就想到 dfs 所以就寫 dfs。

```cpp
const int maxn = 1e5 + 5;
vector<int> g[maxn];
bool vis[maxn];
int dp[maxn], ans;

void dfs(int u)
{   
    vis[u] = 1;
    for (int v : g[u]) 
    {   
        if (!vis[v]) dfs(v);
        dp[u] = max(dp[u], dp[v] + 1);
    }
    ans = max(ans, dp[u]);
}

void solve()
{
    int n, m;
    cin >> n >> m;
    for (int i = 0, u, v; i < m; i++)
    {   
        cin >> u >> v;
        g[u].eb(v);
    }
    for (int i = 1; i <= n; i++)
    {   
        if (!vis[i]) dfs(i);
    }
    cout << ans << endl;
}
```

## H. Grid 1

```cpp
const int maxn = 1000 + 5;
char g[maxn][maxn];
int dp[maxn][maxn];

void solve()
{
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i++)
    {   
        for (int j = 0; j < m; j++)
        {   
            cin >> g[i][j];
        }
    }
    dp[0][0] = 1;
    for (int i = 1; i < n; i++)
    {   
        if (g[i][0] == '.') dp[i][0] = 1;
        else break;
    }
    for (int i = 1; i < m; i++)
    {   
        if (g[0][i] == '.') dp[0][i] = 1;
        else break;
    }
    for (int i = 1; i < n; i++)
    {   
        for (int j = 1; j < m; j++)
        {   
            if (g[i][j] == '.') dp[i][j] = (dp[i - 1][j] + dp[i][j - 1]) % mod;
        }
    }
    cout << dp[n - 1][m - 1] << endl;
}
```

## I. coins

想一下就會發現跟背包很類似，都是可以決定當前這個取不取，然後從上一個狀態轉移過來。

具體來說，定義 $dp_{i, j} :=$ 前 $i$ 個錢幣中有 $j$ 個硬幣向上，所以就可以分成兩個 cases

1. 當前的向上 $\rightarrow dp_{i, j} = p[i] * dp_{i - 1, j - 1}$
2. 當前的向下 $\rightarrow dp_{i, j} = (1 - p[i]) * dp_{i - 1, j}$

最後他要求正多於反，就加一下就好。 btw 邊界條件要設好，我漏設了 debug 好久 QQ

```cpp
const int maxn = 3000 + 5;
double p[maxn], dp[maxn][maxn];

void solve()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) cin >> p[i];
    dp[0][0] = 1.0;
    for (int i = 1; i <= n; i++)
    {   
        dp[i][0] = dp[i - 1][0] * (1 - p[i]);
        for (int j = 1; j <= i; j++)
        {   
            dp[i][j] = p[i] * dp[i - 1][j - 1] +
                        (1 - p[i]) * dp[i - 1][j];
        }
    }
    double ans = 0.0;
    for (int head = (n + 1) / 2; head <= n; head++)
    {   
        ans += dp[n][head];
    }
    cout << fixed << setprecision(12) << ans << endl;
}
```

## J. Sushi

第一次寫到跟期望值有關的 dp，定狀態的方法其實跟其他的也差不多。

這題的定義是 $dp_{i, j, k} :=$ 壽司數量 1, 2, 3 的分別有 $i$, $j$, $k$ 個，把這些都吃到剩 0 的期望值，轉移的話可以看 code

要注意的兩個小重點

1. 要先 run $k$，我看了一些題解不太懂「無後效性」是什麼意思，我的理解是因為一些變成 1 2 的是從 3 來的，所以要先跑
2. 假設當前選的是吃有 2 個盤子，那他會是從 $dp_{i + 1, j - 1, k}$ 轉移過來，代表是多吃一個 2 個的，但因為一次只能吃一個，所以多吃的這個一定是從 1 個的那邊來的

```cpp
const int maxn = 300 + 5;
double dp[maxn][maxn][maxn];
int a[4];

void solve()
{
    int n; 
    cin >> n;
    for (int i = 0; i < n; i++)
    {   
        int x; cin >> x;
        a[x]++;
    }
    for (int k = 0; k <= n; k++)
    {   
        for (int j = 0; j <= n; j++)
        {   
            for (int i = 0; i <= n; i++)
            {   
                if (!i and !j and !k) continue;
                if (i) dp[i][j][k] += i * dp[i - 1][j][k];
                if (j) dp[i][j][k] += j * dp[i + 1][j - 1][k];
                if (k) dp[i][j][k] += k * dp[i][j + 1][k - 1];
                dp[i][j][k] = (dp[i][j][k] + n) / (i + j + k);
            }
        }
    }
    cout << fixed << setprecision(12) << dp[a[1]][a[2]][a[3]] << endl;
}
```
