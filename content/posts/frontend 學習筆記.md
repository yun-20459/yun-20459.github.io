---
title: frontend 學習筆記
date: 2022-07-01 16:50:25
tags: [html, css, emmet, svg]
---

## html

- tag 們可以參考 [這裡](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)

### table tag

簡單來說就是可以生成表格的 tag

- `<tr>`: table row，包在這個裡面的會形成一個 row
- `<td>`: table data，就是放格子中要填的 data
- `<th>`: table header，就是在 table 中的 header
- `<colgroup>`、`<col>`: 前者是拿來放後者的，放幾個就產生幾個 column
  
[例子](https://developer.mozilla.org/en-US/docs/Learn/HTML/Tables/Basics)，他的 [live demo](https://mdn.github.io/learning-area/html/tables/basic/timetable-fixed.html)

- `<caption>`: 字面上意思，會出現在表格上面，可以簡短說明表格內容的文字
- `<thead>`、`<tfoot>`、`<tbody>`: 讓整個 table 結構化

[例子](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/spending-record-finished.html)，他的 [live demo](https://mdn.github.io/learning-area/html/tables/advanced/spending-record-finished.html)

- `id`、`header`: 可以在 table header 上面加上 id，然後在他的子表格（？）透過這些 id 標明他的 header 有誰

[例子](https://github.com/mdn/learning-area/blob/main/html/tables/advanced/items-sold-headers.html)，他的 [live demo](https://mdn.github.io/learning-area/html/tables/advanced/items-sold-headers.html)

## css

- [css cheat sheet](https://websitesetup.org/wp-content/uploads/2019/11/wsu-css-cheat-sheet-gdocs.pdf)

## emmet

應該算是一種輔助寫 html 的強大 plugin

[emmet cheet sheet](https://docs.emmet.io/cheat-sheet/)

### Wrap with Abbreviation

在 vscode 裡的用法是

1. 選起來要被包住的內容
2. 先 `Ctrl + Shift + P` 叫出 command palette
3. 輸入 `Emmet: Wrap with Abbreviation`
4. 然後就可以打指令了

（或者也可以 `command+k` `command+s` 打開 keyboard shortcut 設定，如果懶得設定，可以直接去 extension 下載 emmet keybindings 用）

例如說從

```html
<div id="page">
    <p>Hello world</p>
</div>
```

變成

```html
<div id="page">
    <div class="wrapper">
        <h1>Title</h1>
        <div class="content">
        <p>Hello world</p>
        </div>
    </div>
</div>
```

就是輸入 `div.wrapper>h1{Title}+div.content`

[這邊](https://docs.emmet.io/actions/wrap-with-abbreviation/) 有完整的介紹有哪些好用功能

### Remove tags

[這邊](https://docs.emmet.io/actions/remove-tag/) 就是講說可以直接拔掉 tag 然後他會幫你調整 indent

## svg

一種由 XML 構成的向量圖檔，適合簡單的圖用的格式，Adobe Illustrator 跟 [Figma](https://www.figma.com) 是比較常見用來產 svg 檔的 app，也可以用 [code](https://svgjs.dev/docs/3.0/) 產生。如果想做資料視覺化的 svg，可以用 [這個](https://d3js.org)

這裡有一個插在 html 中的 [例子](https://codepen.io/TheOdinProjectExamples/pen/NWaGdmL)

一些有關裡面的 code 的重點：

- `xmlns`: XML namespace，類似告訴瀏覽器要怎麼解讀這個 xml 檔的命名空間
- `viewBox`: svg 檔的邊界，在 svg 檔裡的元素都是參考這個
- `class`、`id`: 跟 html 的一樣，可以方便 css 或 js 拿東西（或者 [`<use>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/use) 複製東西）
- `<circle>`、`rect`、`path`、`text`: 存在 namespace 的元素，更多可以看 [這裡](https://developer.mozilla.org/en-US/docs/Web/SVG/Element)
- `fill`、`stroke`: 可以用 css 改變的屬性，可以看 [這裡](https://css-tricks.com/svg-properties-and-css/)

### 插入 svg

1. inline: 就是把 xml code 直接貼到 html 內
2. link: 可以用 `<img>`，或者在 css 中寫 `background-image: url(./my-image.svg)`

### svg icon libraries

[Material icons](https://fonts.google.com/icons)

[Feather icons](https://feathericons.com)

[the Noun Project](https://thenounproject.com/browse/icons/term/free/?iconspage=1)
