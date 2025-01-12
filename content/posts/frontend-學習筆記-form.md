---
title: frontend 學習筆記 - form
date: 2022-07-08 14:08:29
tags: [html]
---

## 建立 form

<<<<<<< Updated upstream
<<<<<<< Updated upstream
[範例 code](https://codepen.io/snow-ham1949/pen/abYNBrz)
=======
[範例 code](https://codepen.io/yun-20459/pen/abYNBrz)
>>>>>>> Stashed changes
=======
[範例 code](https://codepen.io/yun-20459/pen/abYNBrz)
>>>>>>> Stashed changes

- `<form>`
  - `action`: 後面要放的是 url，代筆要送資料去處理的地方
  - `method`: 送這個 form 的 http request method
- `<input>`
  - `type`: 輸入的形式，可以是
    - radio: 圓圓的選擇按鍵
      - checked: 預設選起來的選項
    - checkbox: 可以打勾的清單符號
      - checked: 預設選起來的選項
    - text: 文字
    - number: 數字
    - email: 就是 email
    - password: 會自動把使用者輸入用 * 遮掉
    - date: 日期
    - url: 網址
  - `placeholder`: 在輸入框內提示你的輸入應該會長怎樣
  - `name`: 資料放在 form control 後的 reference
  - `required`: 必須填寫
  - `title`: 可以提供 validation msg
- `<label>`
  - `for`: 這個屬性要跟搭配的 `<input>` 的 id 一樣
- `<textarea>`: 給使用者寫文字的地方，可以調整長寬也可以預先輸入文字
- `<select>`: 選單
  - `<option>`: 就是選單裡面的選項
    - `value`: 會傳到 server 的值
    - `selected`: 預設選起來的選項
  - `<optgroup>`: 選單裡面的大類別
    - `label`: 顯示給使用者看的大類別
- `<button>`
  - `type`
    - submit: 預設值，會送出 form
    - reset: 重設 form
    - button: 最 general 的一種
- `<fieldset>`: 可以把相關的 form 弄在一起
- `<legend>`: fieldset 中顯示給使用者看這個 fieldset 的主題

## form 驗證

- text validation
  - `minlength`
  - `maxlength`
  - `pattern`: 吃 [regex](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet) 來判斷 ([regex 另一份教學](https://towardsdatascience.com/regular-expressions-clearly-explained-with-examples-822d76b037b4))
- number validation
  - `min`
  - `max`
  
### styling

可以用 `:valid` 跟 `:invalid` 這兩個 pseudo class 來做

[form validation UX](https://css-tricks.com/form-validation-ux-html-css/)

### report error

[how to report errors](https://www.nngroup.com/articles/errors-forms-design-guidelines/)
