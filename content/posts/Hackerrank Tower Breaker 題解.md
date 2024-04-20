---
title: "Hackerrank Tower Breaker 題解"
date: 2024-04-20T17:26:21+08:00
draft: false
mathjax: true
tags: [game]
---

真的好不會做這種題喔 QQ

[題目](https://www.hackerrank.com/challenges/tower-breakers-1/problem)

解其實意外的簡單，如果 $m = 1$ 的話那一開始 player 1 就輸了，剩下的情況可以看 $n \mod 2$，因為 player 2 最優的操作就是 player 1 做什麼他就做什麼，所以如果偶數個 tower，那代表最後只剩下高度為 1 的 tower 的時候剛好換到 player 1，所以 player 1 輸，相反的如果有奇數個 tower，那最後只剩下高度為 1 的 tower 的時候換到 player 2，所以 player 2 輸。
