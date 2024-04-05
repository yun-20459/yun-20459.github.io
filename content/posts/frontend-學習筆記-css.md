---
title: frontend 學習筆記 - css
date: 2022-06-24 13:10:30
tags: [css]
---

## 基礎架構

```css
<selector> {
    <property>: <value>;
}
```

## selector

- universal selector

    ```css
    * {
        color: purple;
    }
    ```

- type selector

    ```css
    div {
        color: white;
    }
    ```

- class selector

    ```html
    <div class="alert-text severe-alert">
        Please agree to our terms of service.
    </div>
    ```

    ```css
    .alert-text {
        color: red;
    }
    ```

- id selector

    ```html
    <div id="title">My Awesome 90's Page</div>
    ```

    ```css
    #title {
        background-color: red;
    }
    ```

  - difference between class & ID
    > The major difference between classes and IDs is that an element can only have one ID. An ID cannot be repeated on a single page, and the ID attribute should not contain any whitespace at all.

- grouping selector

    如果有部份宣告一樣的話可以 group 起來一起寫

    ```css
    .read,
    .unread {
        color: white;
        background-color: black;
    }

    .read {
        /* several unique declarations */
    }

    .unread {
        /* several unique declarations */
    }
    ```

- chaining selector

    如果有兩個以上的 class 或者 class + id 的話可以串起來

    ```html
    <div>
        <div class="subsection header">Latest Posts</div>
        <p class="subsection" id="preview">This is where a preview for a post might go.</p>
    </div>
    ```

    ```css
    .subsection.header {
        color: red;
    }

    .subsection#preview {
        color: blue;
    }
    ```

- combinator

  - descendant combinator

    `.ancestor .child` 就是只會選在 class `ancestor` 裡 class 是 `child` 的，例如下面的話就是會選到 B 跟 C

    ```html
    <div class="ancestor"> <!-- A -->
        <div class="contents"> <!-- B -->
            <div class="contents"> <!-- C -->
            </div>
        </div>
    </div>

    <div class="contents"></div> <!-- D -->
    ```

    ```css
    .ancestor .contents {
        /* some declarations */
    }
    ```

## properties

- color & background-color

    賦值方法可以看[這裡](https://www.w3schools.com/cssref/css_colors_legal.asp)

    ```css
    p {
        /* hex example: */
        color: #1100ff;
        /* rgb example: */
        color: rgb(100, 0, 127);
        /* hsl example: */
        color: hsl(15, 82%, 56%);
    }
    ```

- font & text
  - font-family
    - font family name ex. "Times New Roman"
    - generic family name ex. sans-serif
    - 可以給 list 讓瀏覽器從第一個去找可以用的 ex. `font-family: "Times New Roman", sans-serif;`
    - web safe 的字體可以參考[這裡](https://www.w3schools.com/cssref/css_websafe_fonts.asp)
    - 想用其他開源字體的話[這裡](https://www.bitdegree.org/learn/font-family-css#how-to-use-a-downloaded-font)
  - font-size ex. `font-size: 22px`
  - font-weight: 類似粗體的概念，給數值的話可以從 1~1000

    ```css
    font-weight: bold /*equivalent of font-weight: 700*/
    ```

  - text-align: 各種對齊方法可以看[這裡](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-align)

- image
  
  下面的例子是假設原圖是 500(h)x1000(w)，這樣寫的話 height 就會變成 250px

    ```css
    img {
        height: auto;
        width: 500px;
    }
    ```

## cascade of CSS

瀏覽器有可能會有 default style 就會導致自己寫了 css 但是出來跟預期的不一樣

- specificity
  ID selector > class selector > type selector

    ```css
    /* rule 1 */
    .subsection {
        color: blue;
    }

    /* rule 2 */
    /* this will be chosen */
    .main .list {
        color: red;
    }
    ```

    ```css
    /* rule 1 */
    #subsection .list {
        background-color: yellow;
        color: blue;
    }

    /* rule 2 */
    /* this will be chosen */
    /* but background will be yellow since there is no conflict declaration for it*/
    #subsection .main .list {
        color: red;
    }
    ```

- inheritance

    沒講的東西就會繼承，但如果有 selector 單指且跟繼承有衝突的話以 selector 為優先

- rule order

    如果有好幾個 class 且優先度都一樣，會選在 css 裡面最後 defined 的那個

## adding CSS into HTML

- external css: 寫個 `style.css` link 進 .html
- internal css: 在 html 的 head 裡寫

    ```html
    <head>
        <style>
            div {
            color: white;
            background-color: black;
            }

            p {
            color: red;
            }
        </style>
    </head>
    <body>...</body>
    ```

## box model

所有在 webpage 上的東西都是 rectangle box，然後可以透過 css 調整這個 box 的特性例如

- padding
  - 如果上下一樣 8px ，左右一樣 24px 的話可以寫 `padding: 8px 24px;`
- margin
  - 想要 element 靠右可以寫 `margin-left: auto;`
  - 想要 element 置中可以寫 `margin: 0 auto;`
- border

![示意圖](https://i.imgur.com/twaMAWQ.png)

## block v.s. inline

- 如果 element 是 block，那就會像一個一個 block 在網頁上按照順序排好，如果是 inline（ex. `<a>`、`<button>`），就會直接在放的位置旁邊的 element 的旁邊（繞口令 xD）
- div & span
  - div 是 block-level [例子](https://codepen.io/TheOdinProjectExamples/pen/KKXXbwR)
  - span 是 inline-level [例子](https://codepen.io/TheOdinProjectExamples/pen/abLLPor)
- [inline-block](https://www.digitalocean.com/community/tutorials/css-display-inline-vs-inline-block)
