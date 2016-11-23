---
title: ES6 的 12 个核心功能一览
tags:
  - JavaScript
  - ES6
date: 2016-11-23 11:49:39
---


过去几年 JavaScript 发生了很大的变化。下面的 12 个新功能现在就可以用起来了。

# JavaScript 历史

新补充的语言叫 ECMAScript 6，也叫 ES6 或 ES2015+。  

JavaScript 自 1995 年面世以来，一直在缓慢地改进着。每隔几年都会有新的补充。1997 年成立的 ECMAScript 引领着 JavaScript 的发展。已发布的版本有 ES3、 ES5、 ES6 等。

![](/images/20161123/history-javascript-evolution-es6.png)

# JavaScript 进化史

ES3 与 ES5 之间隔了 10 年，而ES5 与 ES6 之间隔了 6 年。改进的新模式是每年都渐进式地做一些小改动，而不是像 ES6 一样一次性地进行大量的更改。  

浏览器支持  

所有的现代浏览器和环境都已经支持 ES6 了。  

![](/images/20161123/es6-javascript-support.png)


来源：[https://kangax.github.io/compat-table/es6/](https://kangax.github.io/compat-table/es6/)

Chrome、MS Edge、Firefox、Safari、Node和其它很多环境都已经嵌入程序以支持 JavaScript ES6 的大部分功能。所以从本教程学到的所有功能都可以直接使用。  

来开始学习 ECMAScript 6 吧！

# ES6 核心功能

可以在浏览器控制台上尝试下面的代码。  

![](/images/20161123/javascript-es6-classes-on-browser-console.png)

所以不要照单全收，试试每个 ES5 和 ES6 示例。我们开始吧。

## 块级作用域变量

在 ES6 中，我们使用 let/const，而非 var 来声明变量。  

var 有什么缺点呢？  

var 的问题是变量会泄露到其它代码块，比如 for 循环或是 if 块。  

test(false) 应该返回 outer，但并不是，得到的值是 undefined。  

为什么？  

因为即使 if 块没有执行，第 4 行的表达式 var x 也被提升了。  



```javascript
/*
var 提升：

var 是在函数作用域中的，即使在声明前，它在整个函数内都是可用的。
声明被提升了。所以变量在声明前就可以使用。
初始化 没有 被提升。如果使用 var 的话，一定 要在顶部声明变量。
应用提升规则后，就好理解代码是如何执行的了：
 */
 
//ES5
var x = 'outer';
function test(inner) {
  var x; // HOISTED DECLARATION
  if (inner) {
    x = 'inner'; // INITIALIZATION NOT HOISTED
    return x;
  }
  return x;
}
```

ECMAScript 2015帮助解决了这个问题：  

```javascript
//ES6
 
let x = 'outer';
function test(inner) {
  if (inner) {
    let x = 'inner';
    return x;
  }
  return x; // gets result from line 1 as expected
}
test(false); // outer
test(true); // inner
```

用 let 替代 var 以便按预期的那样去执行代码。如果没有调用 if 块，变量 x 就不会提升到块外。


> Let 提升 和“temporal dead zone”
> 
> * 在 ES6 中，let 会将变量提升到块顶部（而 非 像 ES5 是函数顶部）。
> * 但* 是在变量声明前引用变量会造成 ReferenceError。
> * let 是块作用域的，不可以在声明前使用。
> * "Temporal dead zone"是块开始直到变量被声明的这段区域。

## IIFE

在介绍 IIFE 之前，我们来看个例子：  

```javascript
//ES5
 
{
  var private = 1;
}
console.log(private); // 1
```

如你所见，private 会发生泄漏。需要使用 IIFE（立即执行函数表达式）将其包起来：  

```javascript
//ES5
 
(function(){
  var private2 = 1;
})();
console.log(private2); // Uncaught ReferenceError
```

如果看过 jQuery/lodash 或是其它开源项目的代码，你会注意到它们都利用了 IIFE，以避免污染全局环境，而只在全局下定义 _、$或是 jQuery。  

ES6 更工整，不再需要使用 IIFE，只要用块和 let 就可以了：  

```javascript
ES6
 
{
  let private3 = 1;
}
console.log(private3); // Uncaught ReferenceError
```

## Const

如果不希望变量的值再改变，可以使用 const。  

![](/images/20161123/javascript-es6-const-variables-example.png)


> 总之：用 let 和 const 代替 var。
> 
> 使用 const 进行引用；避免使用 var。
> 
> 如果必须重新指定引用，可以用 let 代替 const。

## 文本模板

遇到文本模板时，不必再用嵌套连接了。比如：  

```javascript
//ES5
 
var first = 'Adrian';
var last = 'Mejia';
console.log('Your name is ' + first + ' ' + last + '.');
```

现在可以用 反引号（`） 和字符串插值 ${}：  

```javascript
//ES6
 
const first = 'Adrian';
const last = 'Mejia';
console.log(`Your name is ${first} ${last}.`);
```

## 多行字符串

不必像这样再连接 + n 字符串了：  

```javascript
//ES5
 
var template = '<li *ngFor="let todo of todos" [ngClass]="{completed: todo.isDone}" >n' +
'  <div class="view">n' +
'    <input class="toggle" type="checkbox" [checked]="todo.isDone">n' +
'    <label></label>n' +
'    <button class="destroy"></button>n' +
'  </div>n' +
'  <input class="edit" value="">n' +
'</li>';
console.log(template);
```

ES6 中同样可以用反引号解决：  

```javascript
ES6
 
const template = `<li *ngFor="let todo of todos" [ngClass]="{completed: todo.isDone}" >
  <div class="view">
    <input class="toggle" type="checkbox" [checked]="todo.isDone">
    <label></label>
    <button class="destroy"></button>
  </div>
  <input class="edit" value="">
</li>`;
console.log(template);
```
两段代码会得到完全相同的结果。  

## 解构赋值

ES6 解构非常简明并且好用。看看下面的例子：  

### 获取数组元素

```javascript
//ES5
 
var array = [1, 2, 3, 4];
var first = array[0];
var third = array[2];
console.log(first, third); // 1 3
```

等同于：  

```javascript
//ES6
 
const array = [1, 2, 3, 4];
const [first, ,third] = array;
console.log(first, third); // 1 3
```

### 调换值

```javascript
//ES5
 
var a = 1;
var b = 2;
var tmp = a;
a = b;
b = tmp;
console.log(a, b); // 2 1
```

等同于  

```javascript
ES6
 
let a = 1;
let b = 2;
[a, b] = [b, a];
console.log(a, b); // 2 1
```

### 返回多个值的解构

```javascript
ES5
 
function margin() {
  var left=1, right=2, top=3, bottom=4;
  return { left: left, right: right, top: top, bottom: bottom };
}
var data = margin();
var left = data.left;
var bottom = data.bottom;
console.log(left, bottom); // 1 4
```

在第3行，也可以像这样用数组返回（并保存序列）：  

```javascript
return [left, right, top, bottom];
```

但之后调用时需要考虑返回数据的顺序。  

```javascript
var left = data[0];
var bottom = data[3];
```

ES6 中调用时只会选择需要的数据（第 6 行）：  

```javascript
//ES6
 
function margin() {
  const left=1, right=2, top=3, bottom=4;
  return { left, right, top, bottom };
}
const { left, bottom } = margin();
console.log(left, bottom); // 1 4
```

注意：第3行用到了一些其它的 ES6 功能。可以将 { left: left } 简化为 { left }。看看和 ES5 的版本相比，现在多简洁啊~很酷不是吗？  

### 参数匹配解构

```javascript
//ES5
 
var user = {firstName: 'Adrian', lastName: 'Mejia'};
function getFullName(user) {
  var firstName = user.firstName;
  var lastName = user.lastName;
  return firstName + ' ' + lastName;
}
console.log(getFullName(user)); // Adrian Mejia
```

等同于（但更简洁）：  

```javascript
//ES6
 
const user = {firstName: 'Adrian', lastName: 'Mejia'};
function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`;
}
console.log(getFullName(user)); // Adrian Mejia
```

### 深度匹配

```javascript
ES5
 
function settings() {
  return { display: { color: 'red' }, keyboard: { layout: 'querty'} };
}
var tmp = settings();
var displayColor = tmp.display.color;
var keyboardLayout = tmp.keyboard.layout;
console.log(displayColor, keyboardLayout); // red querty
```

等同于（但更简洁）：

```javascript
ES6
 
function settings() {
  return { display: { color: 'red' }, keyboard: { layout: 'querty'} };
}
const { display: { color: displayColor }, keyboard: { layout: keyboardLayout }} = settings();
console.log(displayColor, keyboardLayout); // red querty
```

也叫对象解构。  

如你所见，解构很有用，并有助于形成好的编码风格。

> 最佳实践：
> 
> * 使用数组解构获取元素或调换变量，这样就不用创建临时引用了。
> * 对于多返回值的情况，不要用数组解构，用对象解构。

## 类和对象

ES6 用“类”替代“构造函数”。 

> 在 JavaScript 中，每个对象都有原型对象。所有 JavaScript 对象都从原型上继承方法和属性。

ES5 以面向对象编程（OOP）的方式创建对象，是利用构造函数实现的：  

```javascript
ES5
 
var Animal = (function () {
  function MyConstructor(name) {
    this.name = name;
  }
  MyConstructor.prototype.speak = function speak() {
    console.log(this.name + ' makes a noise.');
  };
  return MyConstructor;
})();
var animal = new Animal('animal');
animal.speak(); // animal makes a noise.
```

ES6 提供了语法糖，可以用 `class`、`constructor` 等新的关键字、更少的样板代码实现相同的效果。同样可以看到相比于`constructor.prototype.speak = function ()`，用 `speak()` 定义方法更加清晰：  

```javascript
ES6
 
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}
const animal = new Animal('animal');
animal.speak(); // animal makes a noise.
```

可以看到两种方式（ES5/6）的结果和使用方式相同。 

> 最佳实践：
> 
> * 最好使用 class 语法，避免直接操作 prototype。原因是这样代码更加简明易懂。
> * 避免出现空的构造器。如果没有指明，类会有默认的构造器的。

### 继承

基于前面的 Animal 类，现在想要拓展 Animal，定义一个 Lion 类。  

ES5 原型继承的方式有些复杂。  

```javascript
ES5
 
var Lion = (function () {
  function MyConstructor(name){
    Animal.call(this, name);
  }
  // prototypal inheritance
  MyConstructor.prototype = Object.create(Animal.prototype);
  MyConstructor.prototype.constructor = Animal;
  MyConstructor.prototype.speak = function speak() {
    Animal.prototype.speak.call(this);
    console.log(this.name + ' roars ');
  };
  return MyConstructor;
})();
var lion = new Lion('Simba');
lion.speak(); // Simba makes a noise.
// Simba roars.
```

在此我没有详细解读所有的代码，但是需要注意：  

* 第 3 行，明确地用参数调用 Animal 构造器。
* 7-8 行，将 Lion 原型赋值为 Animal的原型。
* 11 行，从父类 Animal 调用了 speak 方法。

ES6 提供了新的关键字 extends 和 super。  

```javascript
ES6
 
class Lion extends Animal {
  speak() {
    super.speak();
    console.log(this.name + ' roars ');
  }
}
const lion = new Lion('Simba');
lion.speak(); // Simba makes a noise.
// Simba roars.
```

效果相同，但是相比于 ES5，ES6 代码更易读，完胜

> 最佳实践：
> 
> * 使用内置的 extends 实现继承。

## 原生 Promise

用 promise 替代回调地狱

```javascript
ES5
 
function printAfterTimeout(string, timeout, done){
  setTimeout(function(){
    done(string);
  }, timeout);
}
printAfterTimeout('Hello ', 2e3, function(result){
  console.log(result);
  // nested callback
  printAfterTimeout(result + 'Reader', 2e3, function(result){
    console.log(result);
  });
});
```

这个函数接收一个回调，在 done 后执行。我们想要先后执行两次，所以在回调中又一次调用了 printAfterTimeout。  

如果需要 3 或 4 次回调，代码很快就一团糟了。那么用 promise 如何实现呢：

```javascript
ES6
 
function printAfterTimeout(string, timeout){
  return new Promise((resolve, reject) => {
    setTimeout(function(){
      resolve(string);
    }, timeout);
  });
}
printAfterTimeout('Hello ', 2e3).then((result) => {
  console.log(result);
  return printAfterTimeout(result + 'Reader', 2e3);
}).then((result) => {
  console.log(result);
});
```

promise 中可以用 then 在某个函数完成后执行新的代码，而不必再嵌套函数。

## 箭头函数

ES6 没有移除函数表达式，但是新增了箭头函数。  

ES5 中，this 的指向有问题：

```javascript
ES5
 
var _this = this; // need to hold a reference
$('.btn').click(function(event){
  _this.sendData(); // reference outer this
});
$('.input').on('change',function(event){
  this.sendData(); // reference outer this
}.bind(this)); // bind to outer this
```

在函数内，需要用临时变量指向 this 或者使用 bind 绑定。ES6 中可以使用箭头函数。  

## For…of

最开始用 for ，然后使用 forEach，而现在可以用 for…of：  

```javascript
ES6
 
// this will reference the outer one
$('.btn').click((event) =>  this.sendData());
// implicit returns
const ids = [291, 288, 984];
const messages = ids.map(value => `ID is ${value}`);
```

ES6 的 for…of 也可以用来迭代。

## 默认参数

之前需要检测变量是否定义了，而现在可以指定 default parameters 的值。或许你之前像下面这样写过？  

```javascript
ES5
 
function point(x, y, isFlag){
  x = x || 0;
  y = y || -1;
  isFlag = isFlag || true;
  console.log(x,y, isFlag);
}
point(0, 0) // 0 -1 true 
point(0, 0, false) // 0 -1 true 
point(1) // 1 -1 true
point() // 0 -1 true
```

这可能是检测变量有值或指定默认值的惯用模式，但也存在一些问题：

* 第 8 行，我们传的值是 0, 0 但是得到的是 0, -1
* 第 9 行，传进去 false 但是得到的是 true。


如果默认参数是布尔值或将值设为 0，是没有用的。想知道为什么？我会在下面的 ES6 示例后说明。  

有了 ES6，现在可以用更少的代码实现更好的效果了。

```javascript
ES6
 
function point(x = 0, y = -1, isFlag = true){
  console.log(x,y, isFlag);
}
point(0, 0) // 0 0 true
point(0, 0, false) // 0 0 false
point(1) // 1 -1 true
point() // 0 -1 true
```

注意第 5 行和第 6 行我们拿到了想要的值。ES5 的示例不好用，是因为先要检测 undefined 的值，而 false、 null、 undefined 和 0 都是假的值。我们可以加些代码：

```javascript
ES5
 
function point(x, y, isFlag){
  x = x || 0;
  y = typeof(y) === 'undefined' ? -1 : y;
  isFlag = typeof(isFlag) === 'undefined' ? true : isFlag;
  console.log(x,y, isFlag);
}
point(0, 0) // 0 0 true
point(0, 0, false) // 0 0 false
point(1) // 1 -1 true
point() // 0 -1 true
```

现在当检测 undefined 值时就符合我们的要求了。

## 展开操作符

之前使用 arguments，而现在可以用展开操作符。  

ES5 中处理不定参数很麻烦：

```javascript
ES5
 
function printf(format) {
  var params = [].slice.call(arguments, 1);
  console.log('params: ', params);
  console.log('format: ', format);
}
printf('%s %d %.2f', 'adrian', 321, Math.PI);
```

现在可以用展开操作符 ... 达到相同的目的。  

```javascript
ES6
 
function printf(format, ...params) {
  console.log('params: ', params);
  console.log('format: ', format);
}
printf('%s %d %.2f', 'adrian', 321, Math.PI);
```

之前用 apply()，现在可以方便地使用展开操作符 ... 了：

> 提示：apply() 可以将数组转化为一系列参数。例如 Math.max() 接收一系列参数，但如果想应用于数组的话可以用 apply 帮助实现。

![](/images/20161123/javascript-math-apply-arrays.png)

如上所述，apply 可以将数组当作参数序列进行传递：  

```javascript
ES5
 
Math.max.apply(Math, [2,100,1,6,43]) // 100
```

ES6 可以用展开操作符：

```javascript
ES6
 
Math.max(...[2,100,1,6,43]) // 100
```

之前用 concat 合并数组，现在也可以用展开操作符：

```javascript
ES5
 
var array1 = [2,100,1,6,43];
var array2 = ['a', 'b', 'c', 'd'];
var array3 = [false, true, null, undefined];
console.log(array1.concat(array2, array3));
```

ES6 可以用展开操作符展开嵌套的数组：  

```javascript
ES6
 
const array1 = [2,100,1,6,43];
const array2 = ['a', 'b', 'c', 'd'];
const array3 = [false, true, null, undefined];
console.log([...array1, ...array2, ...array3]);
```

# 总结

JavaScript 已经发生了许多改变。本文包含了大部分的核心功能，这些是每个 JavaScript 程序员都应该知道的。本文还涉及了一些最佳实践，可以使你的代码更加简明易懂。  

如果你觉得还有一些其它的必会功能，请在下方留言，方便我更新本文。  

原文链接：[http://web.jobbole.com/88910/](http://web.jobbole.com/88910/)