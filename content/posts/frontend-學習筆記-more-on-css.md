---
title: frontend 學習筆記 - more on css
date: 2022-07-05 19:22:15
tags: [css]
---

## Browser default style

每個瀏覽器會有自己的 default style，所以網路上有開源的 .css 讓開發者們用，可以 undo browser default style 然後根據自己的規則在不同的瀏覽器上顯示一樣的東西

常用的有

1. [Meyer Reset](https://meyerweb.com/eric/tools/css/reset/)
2. [Normalize CSS](https://nicolasgallagher.com/about-normalize-css/)

[這邊](https://browserdefaultstyles.com) 可以看到瀏覽器的 default style

## Absolute/Relative Unit

- Absolute
    像是 px 這種直接給定幾個 pixel 的

- Relative
  - em/rem
    - em: 看元素的 font-size，如果元素的 font-size 是 10px，那 2em = 10px * 2 = 20px
    - rem(recommended): 看 `:root` 或 `html` 的 font-size
  - viewport Unit
    - vh: 1vh = 1% viewport height
    - vw: 1vw = 1% viewport width
    - [viewport unit 可以做的事](https://css-tricks.com/fun-viewport-units/)

## font

- online font library
  - [Google Fonts](https://fonts.google.com)、[Font library](https://fontlibrary.org)
  - 使用

    ```html
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
    ```

    或者

    ```css
    /* style.css */
    @import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
    ```

## text styles

- 如果只是想讓字變斜體用 `font-style`，文字要斜體又加強重點的話用 `<em>`
- 控制文字橫間距用 `letter-spacing: .5em`，行間距用 `letter-height: 1.5`
- `text-transform` 可以變內容全部變大/小寫
- `text-shadow` 可以幫文字加陰影
- `ellipsis` 可以讓超過範圍的文字隱藏，搭配使用如下

    ```css
    .overflowing {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
    ```

    其中 `overflow` 也可以設定成 `visible`、`scroll` 之類的

## advanced selectors

### Parent and Sibling Combinators

- `>`: child combinator
- `+`: adjacent sibling combinator
- `~`: general sibling combinator

例子

```html
<main class="parent">
  <div class="child group1">
    <div class="grand-child group1"></div>
  </div>
  <div class="child group2">
    <div class="grand-child group2"></div>
  </div>
  <div class="child group3">
    <div class="grand-child group3"></div>
  </div>
</main>
```

```css
/* This rule will only select divs with a class of child */
main > div {
  /* Our cool CSS */
}

/* This rule will only select divs with a class of grand-child */
main > div > div {
  /* More cool CSS */
}

/* This rule will only select the div with the class child group2 */
.group1 + div {
  /* Our cool CSS */
}

/* This rule will only select the div with the class child group3 */
.group1 + div + div {
  /* More cool CSS */
}

/* This rule will select all of .group1's siblings - in this case the 2nd and 3rd .child divs */
.group1 ~ div {
  /* Our cool CSS */
}
```

### pseudo class/element

pseudo class 就是讓特定的 html tag 不用寫 class 來加上 css，為什麼不寫 class 是因為可能想指定的是文章第一段，那文章一直增加 html 就要一直改

常用的 pseudo class 有

- `:root`
<<<<<<< Updated upstream
<<<<<<< Updated upstream
- `:first-child`、`:last-child`、`:only-child`、`:empty`（沒小孩）、`:nth-child`[用法](https://codepen.io/snow-ham1949/pen/YzayJgr)
=======
- `:first-child`、`:last-child`、`:only-child`、`:empty`（沒小孩）、`:nth-child`[用法](https://codepen.io/yun-20459/pen/YzayJgr)
>>>>>>> Stashed changes
=======
- `:first-child`、`:last-child`、`:only-child`、`:empty`（沒小孩）、`:nth-child`[用法](https://codepen.io/yun-20459/pen/YzayJgr)
>>>>>>> Stashed changes
- `:invalid`
- `:hover`、`:visited`、`:focus`、`:link`

pseudo element 也是類似的意思，只是它會指的東西是 tag 裡的內容，例如段落的第一行之類的，也可以同時跟 pseudo class 並用

常用的 pseudo element:

- `::before`、`::after`: 可以在裡面設定 `content` 顯示在元素的前後，或者 `content` 為空字串放 block 之類的，可以參考 [這個例子](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Pseudo-classes_and_pseudo-elements)
- `::marker` 可以調整 `<li>` 的 style
- `::first-letter`、`::first-line`
- `::selection`: 可以讓使用者在用滑鼠選文字的時候出現想要的 css style

### attribute selectors

- `[attribute]`: 選所有有這個 attribute 的
- `selector[attribute]`: 選這些 selector 有 這個 attribute 的
- `[attribute="value"]`: 選有這個 attribute 且 value 是特定的

例子

```css
[src] {
    /* This will target any element that has a src attribute. */
}

img[src] {
    /* This will only target img elements that have a src attribute. */
}

img[src="puppy.jpg"] {
    /* This will target img elements with a src attribute that is exactly "puppy.jpg" */
}
```

搭配 regex 使用的話

- `[attribute^="value"]`: value 前綴有 "value" 的
- `[attribute$="value"]`: value 後綴有 "value" 的
- `[attribute*="value"]`: value 中任何地方有出現 "value" 的

## Positioning

1. static: 會被放在預設的位置
2. relative: 跟 static 差不多但是可以定義 top, bottom, left, right
3. absolute: 透過定義 top, bottom, left, right 放在螢幕上的固定位置，適用的 case 有
   - modals
   - image with a caption on it
   - icons on top of other elements
4. fixed: 會直接固定在螢幕上，不管怎麼滑動都會在那裡 ex. 導覽列、聊天按鍵
5. sticky: 還沒滑到之前跟一般元素一樣，滑經過後會變成 fixed

## CSS functions

- `calc()`: 可以透過 css variable 去算各個區塊的大小 [例子](https://codepen.io/TheOdinProjectExamples/pen/OJxNxya)
- `min()`、`max()`: 選最小/大的 [例子](https://web.dev/i18n/en/min-max-clamp/)
- `clamp()`: 要給三個參數，smallest value, ideal value, largest value
- [其他](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Functions)

## custom properties

- CSS variable declaration: `--` 為開頭，用 `-` 來分開單字 ex.`--color-error-text`
- 跟 js 一樣有 scope 的概念，可以繼承，如果是全部都可以用的話可以定義在 `:root`
- 實際應用
  - set themes: `:root` 可以加上 class，再加上一點 js 就可以轉換 theme [例子](https://codepen.io/TheOdinProjectExamples/pen/PojVRMb)
  - default theme: 可以用 `prefers-color-scheme` 這個 media query 來看 OS/user agent 的偏好主題來設定 [例子](https://codepen.io/TheOdinProjectExamples/pen/powGZzE)

## 其他

- `box-shadow`: 元素加上陰影
- [Can I use](https://caniuse.com) 可以看 browser 有沒有支援想寫的 css feature
- 常用的 css framework: [Bootstrap](https://getbootstrap.com)、[Tailwind](https://tailwindcss.com)
- 常用的 css preprocessor: [SASS](https://sass-lang.com)、[Stylus](https://stylus-lang.com)
  - 可以把 code 轉成 css
