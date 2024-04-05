---
title: frontend 學習筆記 - grid
date: 2022-07-10 15:03:48
tags: [csss]
---

## grid 是什麼

一種 display 的方法，可以 display grid （廢話）

## 常用 property & function

- `grid-template-columns`、`grid-template-rows`: 設定有幾行/列，並給訂每一行/列的寬度
  - 給的時候可以用 `repeat(次數, 寬度)` 來給，其中寬度的單位可以是 `px` 或 `fr` (fraction unit)
- `grid-auto-rows`、`grid-auto-columns`: 設定 template 以外的行/列
- `grid-auto-flow`: 指定多出來的 grid 要往哪裡長
- `gap`: 可以同時設定 `row-gap`、`column-gap`
- `resize: both`: 可以讓 user 調整 grid container 的大小
- `overflow: auto`: 如果 container 變太小就可以自動調整成滾動式內容
- `minmax()`: 只能用在 `grid-template-columns`、`grid-template-rows`、`grid-auto-rows`、`grid-auto-columns`，給 container shrink and grow 最小和最大值
- `auto-fit`、`auto-fill`: 會自動依據 container 的大小調整個數，兩個的差別在如果 grid 大到可以容納其他 item 但沒有其他 item 的時候， `auto-fit` 會用 max， `auto-fill` 會用 min
  - `auto-fit` [例子](https://codepen.io/TheOdinProjectExamples/pen/mdByJyJ)
  - `auto-fill` [例子](https://codepen.io/TheOdinProjectExamples/pen/KKXwpwX)

## positioning

- `grid-column-start`、`grid-column-end`、`grid-row-start`、`grid-row-end`: 可以固定透過 line 固定 grid cell 的位置
  - 可以直接用 `grid-area` 來表示 `grid-row-start / grid-column-start / grid-row-end / grid-column-end`
- `grid-area` 也可以直接給 value，然後再用 `grid-template-area` 排，空的地方就用 `.` 代表
  - [例子](https://codepen.io/TheOdinProjectExamples/pen/ZEXYbpg)
- `span`: 這個 keyword 可以直接指定 number of track to span [詳細解說](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Line-based_Placement_with_CSS_Grid#using_the_span_keyword)
- [other css tricks](https://css-tricks.com/snippets/css/complete-guide-grid/#grid-properties)
- [相關練習](https://cssgridgarden.com)
