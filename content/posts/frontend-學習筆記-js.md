---
title: frontend 學習筆記 - js
date: 2022-06-26 15:05:15
tags: [js]
---

## 寫在網頁裡的方法

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Page Title</title>
</head>
<body>
  <script>
    // Your JavaScript goes here!
    console.log("Hello, World!")
  </script>
</body>
</html>
```

或者

```html
<script src="javascript.js"></script>
```

## Variable

### 宣告

- `let` 是一般變數，`const`是常數變數
- 用駝峰式命名法（const 的話全部大寫）
- `-` 不應該出現在命名中，無意義的變數可以用 `$` 或者 `_`
- 不要把變數都宣告在同一行
- 可以用科學記號賦值
- integer 精度只到 15 位數

    ```javascript
    let x = 999999999999999;   // x will be 999999999999999
    let y = 9999999999999999;  // y will be 10000000000000000
    ```

### string method

#### Extract string part

- slice: 區間是左閉右開

    ```javascript
    let str = "Apple, Banana, Kiwi";
    alert(str.slice(7, 13)); // "Banana"
    alert(str.slice(-12, -6)); // "Banana"
    alert(str.slice(7)); // "Banana, Kiwi"
    alert(str.slice(-12)); // "Banana, Kiwi"
    ```

- substring: 跟 slice 很像只是不吃負數
- substr: 跟 slice 很像只是第二個參數是長度，不給的話就是剩下全部
  
#### replace string content

- replace
  - example code

    ```javascript
    let text = "Please visit Microsoft!";
    let newText = text.replace("Microsoft", "W3Schools");
    ```

  - 只會 replace 第一個出現的 (可以用正規表達式弄成全部替換)

    ```javascript
    let text = "Please visit Microsoft and Microsoft!";
    let newText = text.replace(/Microsoft/g, "W3Schools");
    ```

  - case sensitive (可以用正規表達式弄成 insensitive)

    ```javascript
    let text = "Please visit Microsoft!";
    let newText = text.replace(/MICROSOFT/i, "W3Schools");
    ```

  - 會回傳一個新的 string 而不是去改原 string

#### Convert to Upper/Lower case

- `toUpperCase()`、`toLowerCase()`

#### Extract string character

- `charAt()` 跟 `[]` 可以直接拿特定位置的字元，`charCodeAt()` 是拿特定字元的 unicode

#### string 轉 array

- `split`: 看要用什麼字串把 string 切開變成 array，如果字串給的是 `""`，那就會把每個字元都切開

#### Others

- `trim()` 可以剪掉前後的空白字元
- `padStart()`、`padEnd()` 可以在 string 前/後 padding，第一個參數是 padding 過後的總長，第二個參數是要 padding 的字串
  
    ```javascript
    let text = "5";
    let padded = text.padStart(4,"0"); // "0005"
    let padded = text.padEnd(4,"x"); // "5xxx"
    ```

## 運算子

### 算術運算子

跟 C++ 不一樣的是指數是 `**` 剩下都一樣。

不過加法運算子 `+` 比較酷，看以下例子可以發現數字 + string 會是 string concatenation，但是有些情況不一樣，我猜應該是看前後型態一不一致，如果不一樣才轉。

```javascript
let x = 10;
let y = "20";
let z = x + y; // "1020"
```

```javascript
let x = 10;
let y = 20;
let z = "The result is: " + x + y;
// The result is 1020;
```

```javascript
let x = 10;
let y = 20;
let z = "30";
let result = x + y + z; // "3030"
```

剩下一些酷酷的東西

- 可以直接拿 string -/*，要轉成數字 + 的話可以這樣寫

    ```javascript
    let string1 = "1";
    let string2 = "2";
    alert( +string1 + +string2 ); // "3"
    ```

- 可以用 `isNaN()` 來看一個變數是不是 NaN (NaN: value is not a number)
- js 預設是以十進位 display number，如果要用其他進位可以用 `.toString(<base>)`

### 比較運算子

`==` 會先把變數轉成同個 type 再比對，`===` 是直接比對。

```javascript
let number2 = 2;
let string2 = "2";
alert(number2 == string2); // true
alert(number2 === string2); // false
```

### 邏輯運算子

- `&&` 如果第一個變數是 truthy，那就會回傳第二個變數；如果是 falsy，就會回傳第一個變數
- `||` 如果遇到一個 truthy 就會回傳，如果全部都是 falsy，就回傳最後一個

## 語法

`switch` 跟 C++ 的一樣

### array

1. 宣告

    ```javascript
    const array = [];
    array[0] = "hello";
    array[1] = "world";

    // or

    const array = ["hello", "world"];
    ```

2. 取/改值

    ```javascript
    // access array element
    let string = array[0];
    // change an array element
    array[0] = "hi";
    ```

3. 網頁存取整個陣列

    ```javascript
    const cars = ["Saab", "Volvo", "BMW"];
    document.getElementById("demo").innerHTML = cars;
    ```

4. array element 可以是不同 type (variable, function, array 都可以)

5. array v.s. objects
   - array 用的是 numbered indexes
   - object 用的是 named indexes

6. 可以用 `Array.isArray(<array name>)` 來看是不是 array
7. 常用 method
   - `toString()`

        ```javascript
        const fruits = ["Banana", "Orange", "Apple", "Mango"];
        document.getElementById("demo").innerHTML = fruits.toString();
        // Banana,Orange,Apple,Mango
        ```

   - `.join()`

        ```javascript
        const fruits = ["Banana", "Orange", "Apple", "Mango"];
        document.getElementById("demo").innerHTML = fruits.join(" * ");
        // Banana * Orange * Apple * Mango
        ```

   - `.push()`、`.pop()`

        ```javascript

        const fruits = ["Banana", "Orange", "Apple"];
        fruits.push("Lemon");  
        // or
        fruits[fruits.length] = "Lemon";

        let fruit = fruits.pop(); // Lemon
        ```

   - `.shift()`

        把最前面的 element pop 出來然後把剩下的 element 往前推

        ```javascript
        const fruits = ["Banana", "Orange", "Apple", "Mango"];
        let fruit = fruits.shift(); // Banana
        ```

   - `.unshift()`

        在最前面加入元素然後把剩下的往後推

        ```javascript
        const fruits = ["Banana", "Orange", "Apple", "Mango"];
        fruits.unshift("Lemon"); // return new array length
        ```

   - `.concat()`

        concatenate 兩個以上的 array

        ```javascript
        const myGirls = ["Cecilie", "Lone"];
        const myBoys = ["Emil", "Tobias", "Linus"];

        const myChildren = myGirls.concat(myBoys);
        ```

   - `.splice()`

        在指定位置插入元素。
        第一個參數：插入的 index
        第二個參數：插入之前要先刪掉第一個參數開始（包括）的幾個 element
        後面的參數：要插入的元素

        ```javascript
        const fruits = ["Banana", "Orange", "Apple", "Mango"];
        fruits.splice(2, 0, "Lemon", "Kiwi");
        ```

   - `.slice()`

        跟 str 的 slice 很像，就是把指定的 index（包括）後的所有 array 切下來

        ```javascript
        const fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
        const citrus = fruits.slice(1);
        const citrus = fruits.slice(1, 3);
        ```

### loop

- for

    下面有幾種 for 的寫法，其實就跟 C++ 差不多。

    ```javascript
    for (let i = 0; i < 100; i++) {
        // do something here
    }

    const cats = ['Pete', 'Biggles', 'Jasmine'];

    let myFavoriteCats = 'My cats are called ';

    for (const cat of cats) {
    myFavoriteCats += `${cat}, `
    }

    console.log(myFavoriteCats); // "My cats are called Pete, Biggles, Jasmine, "
    ```

- while、do-while: 跟 C++ 一模一樣
- break
  
  js 的 break 還蠻酷的，簡單說可以想成類似幫 loop 取名，然後就可以透過 loop name 直接 break 出去

  ```javascript
    outer: for (let i = 0; i < 3; i++) {

    for (let j = 0; j < 3; j++) {

        let input = prompt(`Value at coords (${i},${j})`, '');

        // if an empty string or canceled, then break out of both loops
        if (!input) break outer; // (*)

        // do something with the value...
    }
    }

    alert('Done!');
  ```

## Function

### anonymous function

簡單來說就是沒有名字的 function，可以看下面這個例子。

原始的寫法：

```javascript
function logKey(event) {
    console.log(`You pressed "${event.key}".`);
}

textBox.addEventListener('keydown', logKey);
```

可以發現其實 function 不需要另外 define，所以可以寫成：

```javascript
textBox.addEventListener('keydown', function(event) {
    console.log(`You pressed "${event.key}".`);
});
```

就變成 pass anonymous function，然後這可以有另一種寫法叫做 arrow function。

```javascript
textBox.addEventListener('keydown', (event) => {
    console.log(`You pressed "${event.key}".`);
});
```

其中如果 code 只有一行大括號可以拿掉，參數只有一個可以把小括號拿掉。

```javascript
textBox.addEventListener('keydown', event => console.log(`You pressed "${event.key}".`));
```

### function expression

意思是說我可以在任何 expression 的中間 create functions.

```javascript
// function expression
let sayHi = function() {
    alert( "Hello" );
};

//function declaration
function sayHi() {
  alert( "Hello" );
}
```

- function is a value
    因為 function 是 value，所以我可以 print 他的值出來看。

    ```javascript
    function sayHi() {
        alert( "Hello" );
    }

    alert( sayHi ); // shows the function code
    ```

    也可以把他 copy 給其他 variable

    ```javascript
    function sayHi() {   // (1) create
        alert( "Hello" );
    }

    let func = sayHi;    // (2) copy

    func(); // Hello     // (3) run the copy (it works)!
    ```

- block scope
    在 strict mode 裡，只要在 block 裡有做 function declaration，那在 block 裡的任何一個地方用都可以。

### arrow functions

function 比較簡單簡潔的寫法，在前面 [anonymous function](#anonymous-function) 有提到過。

如果一個 function 原本寫成這樣

```javascript
let func = function(arg1, arg2, ..., argN) {
    return expression;
};
```

那就可以改成

```javascript
let func = (arg1, arg2, ..., argN) => expression;
```

如果沒有參數的話可以寫成

```javascript
let sayHi = () => alert("Hello!");
```

## Clean code style

1. Indentention 不要混用
2. line length 不要太長
3. 命名的時候變數以名詞跟形容詞做開頭，function 以動詞做開頭
4. function 不要寫太大，試著弄成好幾個小 function 比較好維護 + debug
5. comment 不要亂寫，必要的時候拿來解釋 code 在幹嘛 (code tells you how, comment tells you why)

## Node.js

> Node.js is a JavaScript runtime environment that allows you to run JavaScript outside of your web browser.

1. 下載 nvm (node version manager)

    ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    ```

2. 下載 node

    ```bash
    nvm install --lts
    ```

3. 設定 node 版本

    ```bash
    nvm use --lts
    ```

4. 使用

    打開 terminal 打

    ```bash
    node
    ```

    就可以用了，如果要關掉就打

    ```bash
    .exit
    ```

## DOM (document object model)

把 html 裡的各個標籤都變成物件，然後這些物件會變成樹狀結構的 node。

1. selector
    可以取到特定 node 的東西

    假設 html 長這樣

    ```html
    <div id="container">
        <div class="display"></div>
        <div class="controls"></div>
    </div>
    ```

    我想要拿到 `<div class="display"></div>` 的話，可以用 `div.display`、`.display`、`#container > .display`、`div#container > div.display`

    或者用

    ```javascript
    const container = document.querySelector('#container');
    // selects the #container div (don't worry about the syntax, we'll get there)

    console.dir(container.firstElementChild);                      
    // selects the first child of #container => .display

    const controls = document.querySelector('.controls');     
    // selects the .controls div

    console.dir(controls.previousElementSibling);                  
    // selects the prior sibling => .display
    ```

   - querySelector
        如果用 `.querySelectorAll()` 的話，回傳的會是 nodelist 不是 array！可以用 `Array.from()` 或者 [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) 解決

2. element creation

    `document.createElement(tagName, [options])` 可以在 DOM memory （不是 page）中建一個 tag type tagName 的 element。

    - append element

        ```javascript
        parentNode.appendChild(childNode) 
        // appends childNode as the last child of parentNode
        parentNode.insertBefore(newNode, referenceNode) 
        // inserts newNode into parentNode before referenceNode
        ```

    - remove element

        ```javascript
        parentNode.removeChild(child) 
        // removes child from parentNode on the DOM and returns a reference to child

        ```

    - altering element

        可以改 element 的屬性

        ```javascript
        const div = document.createElement('div');                     
        // creates a new div referenced in the variable 'div'
        div.style.color = 'blue';                                      
        // adds the indicated style rule

        div.style.cssText = 'color: blue; background: white;';          
        // adds several style rules

        div.setAttribute('style', 'color: blue; background: white;');    
        // adds several style rules

        div.setAttribute('id', 'theDiv');                              
        // if id exists, update it to 'theDiv', else create an id
        // with value "theDiv"

        div.getAttribute('id');                                        
        // returns value of specified attribute, in this case
        // "theDiv"

        div.removeAttribute('id');                                     
        // removes specified attribute

        div.textContent = 'Hello World!'                               
        // creates a text node containing "Hello World!" and
        // inserts it in div

        div.innerHTML = '<span>Hello World!</span>';                   
        // renders the HTML inside div
        ```

        要注意的是 css 是 kebab-cased 的話，js 在 assign 屬性要用 camelCase

        ```javascript
        div.style.background-color 
        // doesn't work - attempts to subtract color from div.style.background
        div.style.backgroundColor 
        // accesses the div's background-color style
        div.style['background-color'] 
        // also works
        div.style.cssText = "background-color: white;" 
        // ok in a string
        ```

    - working with classes

        ```javascript
        div.classList.add('new');                                      
        // adds class "new" to your new div

        div.classList.remove('new');                                   
        // removes "new" class from div

        div.classList.toggle('active');                                
        // if div doesn't have class "active" then add it, or if
        // it does, then remove it
        ```

### Event

就是各種會發生在網頁上的事？例如按滑鼠之類的

可以用下列三種方法建立 event

- method 1

    ```html
    <button onclick="alert('Hello World')">Click Me</button>

    // named function method with js
    <button onclick="alertFunction()">Click Me</button>
    ```

    ```javascript
    function alertFunction() {
        alert("YAY! YOU DID IT!");
    }
    ```

- method 2

    ```html
    <button id="btn">Click Me</button>
    ```

    ```javascript
    const btn = document.querySelector('#btn');
    btn.onclick = () => alert("Hello World");

    // named function method with js above
    btn.onclick = alertFunction;
    ```

- method 3

    ```html
    <button id="btn">Click Me</button>
    ```

    ```javascript
    const btn = document.querySelector('#btn');
    btn.addEventListener('click', () => {
        alert("Hello World");
    });

    // named function method with js above
    btn.addEventListener('click', alertFunction);
    ```

可以用這個來做酷酷的動畫，例如說

```javascript
btn.addEventListener('click', function (e) {
  e.target.style.background = 'blue';
});
```

這段 code 中 `e` 是指那個觸發的 event， `.target` 則會拿到被點擊的 DOM node。

- 如果要再多個 button 上面加 EventListener 呢？

    ```html
    <div id="container">
        <button id="1">Click Me</button>
        <button id="2">Click Me</button>
        <button id="3">Click Me</button>
    </div>
    ```

    ```javascript
    // buttons is a node list. It looks and acts much like an array.
    const buttons = document.querySelectorAll('button');

    // we use the .forEach method to iterate through each button
    buttons.forEach((button) => {

        // and for each one we add a 'click' listener
        button.addEventListener('click', () => {
            alert(button.id);
        });
    });
    ```
