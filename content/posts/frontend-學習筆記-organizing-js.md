---
title: frontend 學習筆記 - organizing js
date: 2022-07-12 13:23:03
tags: [js]
---

## creating object

1. 定義物件的時候用 object literal syntax 較好，這個的意思是把 property 跟 value 用類似 array 的方式來寫，例如說

    ```javascript
    const myObject = {
        property: 'Value!',
        otherProperty: 77,
        "obnoxious property": function() {
            // do stuff!
        }
    }
    ```

2. 可以用 dot notation 或者 bracket notation 拿資料

    ```javascript
    // dot notation
    myObject.property // 'Value!'

    // bracket notation
    myObject["obnoxious property"] // [Function]
    ```

3. function 定義在 prototype 比較好，不然每 new 一個 object 出來就會複製一次

    ```javascript
    function Student(name, grade) {
        this.name = name
        this.grade = grade
    }

    Student.prototype.sayName = function() {
        console.log(this.name)
    }
    Student.prototype.goToProm = function() {
        console.log("Eh.. go to prom?")
    }
    ```

4. prototypal inheritance 建議用 `Object.create`

    ```javascript
    function Student() {
    }

    Student.prototype.sayName = function() {
        console.log(this.name)
    }

    function EighthGrader(name) {
        this.name = name
        this.grade = 8
    }

    EighthGrader.prototype = Object.create(Student.prototype)

    const carl = new EighthGrader("carl")
    carl.sayName() // console.logs "carl"
    carl.grade // 8
    ```

5. Factory function

    因為用 constructor 可能會有很多可能會有很多非預期錯誤，所以可以使用 factory function 來 create object

    ```javascript
    const personFactory = (name, age) => {
        const sayHello = () => console.log('hello!');
        return { name, age, sayHello };
    };

    const jeff = personFactory('jeff', 27);

    console.log(jeff.name); // 'jeff'

    jeff.sayHello(); // calls the function and logs 'hello!'
    ```

    - inheritance:

        ```javascript
        const Person = (name) => {
            const sayName = () => console.log(`my name is ${name}`);
            return {sayName};
        }

        const Nerd = (name) => {
            // simply create a person and pull out the sayName function with destructuring assignment syntax!
            const {sayName} = Person(name);
            const doSomethingNerdy = () => console.log('nerd stuff');
            return {sayName, doSomethingNerdy};
        }

        const jeff = Nerd('jeff');

        jeff.sayName(); //my name is jeff
        jeff.doSomethingNerdy(); // nerd stuff
        ```

6. class

    有 getter 跟 setter 可以用

    ```javascript
    class User {

        constructor(name) {
            // invokes the setter
            this.name = name;
        }

        get name() {
            return this._name;
        }

        set name(value) {
            if (value.length < 4) {
            alert("Name is too short.");
            return;
            }
            this._name = value;
        }

    }
    ```

## webpack

打包檔案的模組化好工具，可以看這個人的 [介紹](https://neighborhood999.github.io/webpack-tutorial-gitbook/Part1/)

## SOLID principle

1. [Single Responsibility Principle](https://medium.com/程式愛好者/使人瘋狂的-solid-原則-單一職責原則-single-responsibility-principle-c2c4bd9b4e79)
2. Open-Closed Principle: open to extension but close to modification，舉例來說當我想要擴建某個 function 的時候我應該要可以直接在 import code 那邊可以擴建而不是去更改 import 的 code
3. [Liskov Substitution Principle](https://stackoverflow.com/questions/56860/what-is-an-example-of-the-liskov-substitution-principle#answer-584732)
4. Interface Segregation: export code 的時候 require 必要的東西，剩下的 optional
5. Dependency Inversion Principle: 大概是上層的模組不依賴下層的，而是用 interface 去實現兩個的接口（？） [介紹](https://www.jyt0532.com/2020/03/24/dip/)

## linting

[vscode eslint 教學](https://www.digitalocean.com/community/tutorials/linting-and-formatting-with-eslint-in-vs-code)

## constraint validation

[使用 html + js 做 validation](https://developer.mozilla.org/en-US/docs/Web/API/Constraint_validation)
