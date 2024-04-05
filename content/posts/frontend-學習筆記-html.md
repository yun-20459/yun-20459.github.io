---
title: frontend 學習筆記 - html
date: 2022-06-21 21:26:41
tags: [html]
---

## 學習網站

[link](https://www.theodinproject.com)

## html

- ```<title> </title>``` : 網頁標題

- ```<p> </p>```: 網站內容中的文章 tag
- ```<h1> </h1>```: 標題字，有 h1~h6
- ```<strong> </strong>```: 粗體
- ```<em> </em>```: 斜體
- ```<!-- comment -->```
- list
  - unordered list

    ```html
    <ul>
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
    </ul>
    ```

  - ordered list

    ```html
    <ol>
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
    </ol>
    ```

- ```<a href="url"> </a>```: 超連結
  - url 可以是絕對連結（ex. 外部）or 相對連結（ex. 內部連結）

    ```html
    <body>
        <h1>Homepage</h1>
            <a href="https://www.theodinproject.com/about">click me</a>

            <a href="./about.html">About</a>
    </body>
    ```

- ```<img>```: 圖片，一樣可以用絕對連結或相對連結

    ```html
    <body>
        <h1>Homepage</h1>
            <img src="https://www.theodinproject.com/mstile-310x310.png">

            <img src="images/dog.jpg" alt="alternative text">
    </body>
    ```

## others

- ```open ./index.html```可以直接用 os 預設瀏覽器開啟 index.html
- [replit](https://replit.com) 是 live coding 的好網站
- [html validator](https://validator.w3.org) 可以檢查 html 語法錯誤之類的
