---
title: TIOJ 1882. pC. 最暖的冬天
date: 2022-07-23 21:26:51
tags: [TIOJ, tenary search]
---

## 題目

[link](https://tioj.ck.tp.edu.tw/problems/1882)

## 作法

純純的三分搜，只是我一開始沒看懂題目就開始亂寫，然後又在「求出 $L(x)$ 這部分卡住」，後來學長開導（？）我之後才知道其實就是在三分搜的時候原本是問一個函數，現在他要的是所有函數集合的最小值，那就 $O(n)$ 問過去就好了。

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long int ll;
typedef pair<int, int> pii;
typedef vector<int> vi;
typedef vector<pii> vii;

const int inf = 0x3f3f3f3f;
const int ninf = 0xc0c0c0c0;
const double dinf = numeric_limits<double>::max();
const double dninf = numeric_limits<double>::min();
const double eps = 1e-9;

#define AC ios_base::sync_with_stdio(0), cin.tie(0);
#define rep(i, j, k) for (int i = j; i < k; i++)
#define repb(i, j, k) for (int i = j; i >= k; i--)
#define all(v) (v).begin(), (v).end()
#define endl '\n'
#define F first
#define S second
#define err(a) cerr << #a << ": " << a << endl

typedef struct Func {
    double a, b, c;
} Func;

double f(Func func, double x)
{
    return func.a * x * x + func.b * x + func.c;
}

double triSearch(vector<Func>& vf, double lowest_x, double highest_x)
{
    double low = lowest_x, l, r, high = highest_x;
    int n = vf.size();
    while (high - low > eps)
    {
        l = low + (high - low) / 3, r = l + (high - low) / 3;
        double lv = dinf, rv = dinf;
        rep(i, 0, n)
        {
            lv = min(lv, f(vf[i], l));
            rv = min(rv, f(vf[i], r));
        }
        if (lv < rv) low = l;
        else high = r;
    }

    return low;
}

int main() {

    AC;

    int n; cin >> n;
    vector<Func> v(n);
    rep(i, 0, n)
    {
        cin >> v[i].a >> v[i].b >> v[i].c;
    }
    
    cout << fixed << setprecision(6) << triSearch(v, 0.0, 60.0 * 24.0 * 94.0) << endl;
    
    return 0;
}
```
