---
title: frontend 學習筆記 - flexbox
date: 2022-06-25 13:10:13
tags: [css, flexbox]
---

## flexbox 是什麼

應該是個切版好工具？大概的功能是讓 element 可以併排，而不是 stacking 或者 centering。 例子可以看 [這裡](https://codepen.io/TheOdinProjectExamples/pen/QWgNxrp)

## 分類

| flex container | flex item |
| --- | --- |
| 外容器 | 在 flex container 中的 item |
| display, flex-flow(direction & wrap), justify-content, align-items | flex, order, align-self|

- flex item 也可以同時作為 flex container！

## flex container

### display

可以用 `display: flex;` 或 `display: inline-flex;`，後者是類似 inline-block + flex。

### flex-flow

1. axes

    設定 main axis 跟 cross axis，這兩個東西是相對的。 ex. `flex-direction: row` 就是讓 main axis 是水平的，也就是說會讓 item 水平排列。

    default 是 row，如果變成 column 的話，flex-basis 會看 height 而不是 width。

    - 還可以用 `row-reverse` 跟 `column-reverse`，會影響 start end line，詳細可以看[這裡](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)

2. wrap

    可以用 `nowrap`、`wrap`、`wrap-reverse`決定不換行、換行、換行時反轉

### alignment

如果想要排 main axis 上的東西用 `justify-content`，排 cross axis 上的東西用 `align-items`。東西太擠的話可以加上 `gap`。

- justify-content 選項：flex-start、flex-end、center、space-between、space-around
- align-items 選項：flex-start、flex-end、center、baseline、stretch
  - align-content 選項（多行版本）: flex-start、flex-end、center、space-between、space-around、stretch

可以玩一下這個人的 [codepen](https://codepen.io/Wcc723/pen/VWJPaa) 理解

## flex item

### flex

- flex 裡面分為：`flex-grow`、`flex-shrink`、`flex-basis`
  - default: `flex: 0 1 0%`
  - flex-grow: 就是看在 container 中要長大成幾倍
  - flex-shrink: grow 的相反，不過只會在 item 全部總寬大於 container 的時候才會 apply
  - flex-basis: grow 跟 shrink 的 baseline
    - default 是 `auto`，會去看 item 的 width 是多少
    - 寫 `flex: 1` 等於 `flex: 1 1 0%`
    - 寫 `flex: auto` 等於 `flex: 1 1 auto`

### order

可以按照自己需求排列 item，可以看這個[例子](https://w3c.hexschool.com/flexbox/4007bc24)比較好懂。

### align-self

調整 cross axis 的對齊設定，可以用 auto、flex-start、flex-end、center、baseline、stretch

寫法的話直接

```css
.align-self {
    align-self: center;
}
```
