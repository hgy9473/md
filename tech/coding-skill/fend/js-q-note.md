# JS 常见疑难问题笔记

## 函数执行过程相关

* 闭包
* 作用域链
* 执行上下文
* this 值

**闭包** 其实只是一个绑定了执行环境的函数。**闭包**与普通函数的区别是，它携带了执行的环境。

JavaScript 标准把一段代码（包括函数），执行所需的的信息定义为**执行上下文**。

**执行上下文**在 ES3 中包含 3 个部分：
  
* `scope` : 作用域，也常常被叫叫作用域链。
* `variable object`: 变量对象，用于存储变量的对象。
* `this value`: this 值。

```js

function showThis(){
    console.log(this);
}

var o = {
    showThis: showThis
}

showThis(); // global
o.showThis(); // o


```

调用函数时使用的引用，决定了函数执行时刻的 this 值。

我们获取函数的表达式，它实际上返回的并非函数本身，而是一个而是一个 Reference 类型。

Reference 类型由两部分组成：一个对象和一个属性值。

在这个例子中，Reference 类型中的对象被当作 this 值，传入了执行函数时的上下文当中。

JavaScript 用一个栈来管理执行上下文，这个栈中的每一项又包含一个链表。

当函数调用时，会入栈一个新的执行上下文，函数调用结束时，执行上下文被出栈。

而 this 则是一个更为复杂的机制，JavaScript 标准定义了 *thisMode* 私有属性。

*thisMode* 私有属性有三个取值。

* lexical：表示从上下文中找 this，这对应了箭头函数。

* global：表示当 this 为 undefined 时，取全局对象，对应了普通函数。

* strict：当严格模式时使用，this 严格按照调用时传入的值，可能为 null 或者 undefined。

Function.prototype.call 和 Funcion.prototype.apply 可以指定函数调用时传入的 this 值。

Function.prototype.bind 它可以生成一个绑定过的函数，这个函数的 this 值固定了参数。

