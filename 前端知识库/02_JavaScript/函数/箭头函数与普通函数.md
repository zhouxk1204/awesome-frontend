# JavaScript：箭头函数 vs 普通函数

## 📝 概述

在 JavaScript 中，箭头函数（Arrow Function）和普通函数（Function）在 \`this\` 指向上有显著差异。  
理解两者区别，有助于在实际开发和面试中精准回答函数相关问题。

## 📌 普通函数的 this 指向

普通函数的 \`this\` 是动态的，取决于函数调用方式：

1. **全局调用**

```js
function fn() { console.log(this); }
fn(); // window（浏览器环境）
````

2. **对象方法调用**

```js
const obj = { sayHi: function() { console.log(this); } };
obj.sayHi(); // obj
```

3. **构造函数调用**

```js
function Person(name) { this.name = name; }
const person = new Person('Alice');
console.log(person.name); // Alice
```

4. **call/apply/bind**

```js
function greet() { console.log(this.name); }
const obj = { name: 'Alice' };
greet.call(obj); // Alice
```

5. **事件回调**

```js
document.querySelector('button').onclick = function() {
    console.log(this); // button
};
```

## 📌 箭头函数的 this 指向

箭头函数没有自己的 `this`，继承自外层作用域：

1. **继承外层 this**

```js
const obj = {
    sayHi: () => { console.log(this); } // window
};
obj.sayHi();
```

2. **事件回调中的 this**

```js
document.querySelector('button').onclick = () => { console.log(this); }; // window
```

3. **定时器中的 this**

```js
setTimeout(() => { console.log(this); }, 1000); // window
```

4. **严格模式下**

```js
'use strict';
const obj = { sayHi: () => { console.log(this); } };
obj.sayHi(); // window
```

## ⚡ 核心区别

| 维度      | 普通函数                 | 箭头函数                 |
| - | -- | -- |
| this 指向 | 动态，取决于调用方式           | 静态，继承自外层作用域          |
| 语法      | function 关键字         | => 简洁语法              |
| 适用场景    | 需要动态 this，如对象方法、构造函数 | 不需要动态 this，如回调函数、定时器 |

## 🏷️ 面试回答重点

1. **this 指向**：普通函数动态，箭头函数继承外层作用域
2. **语法**：箭头函数更简洁
3. **适用场景**：普通函数用于动态 this，箭头函数用于固定 this

## 🔗 关联笔记

<!-- JavaScript函数, this, 箭头函数, 普通函数, 面试知识点 -->

## 🏷️ 标签

#javascript #函数 #箭头函数 #普通函数 #this #前端面试
