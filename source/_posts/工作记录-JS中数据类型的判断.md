---
title: JS中数据类型的判断
author: GleanWu
tags:
  - javascript
---

### typeof

typeof 对于原始类型来说，除了 null 都可以显示正确的类型

```javascript
console.log(typeof 2) // number
console.log(typeof true) // boolean
console.log(typeof 'str') // string
console.log(typeof []) // object     []数组的数据类型在 typeof 中被解释为 object
console.log(typeof function() {}) // function
console.log(typeof {}) // object
console.log(typeof undefined) // undefined
console.log(typeof null) // object     null 的数据类型被 typeof 解释为 object
```

typeof 对于对象来说，除了函数都会显示 object，所以说 typeof 并不能准确判断变量到底是什么类型,所以想判断一个对象的正确类型，这时候可以考虑使用 instanceof

### instanceof

instanceof 可以正确的判断对象的类型，因为内部机制是通过判断对象的原型链中是不是能找到类型的 prototype。

```javascript
console.log(2 instanceof Number) // false
console.log(true instanceof Boolean) // false
console.log('str' instanceof String) // false
console.log([] instanceof Array) // true
console.log(function() {} instanceof Function) // true
console.log({} instanceof Object) // true
// console.log(undefined instanceof Undefined);
// console.log(null instanceof Null);
```

可以看出直接的字面量值判断数据类型，instanceof 可以精准判断引用数据类型（Array，Function，Object），而基本数据类型不能被 instanceof 精准判断。
我们来看一下 instanceof  在 MDN 中的解释：instanceof  运算符用来测试一个对象在其原型链中是否存在一个构造函数的  prototype  属性。其意思就是判断对象是否是某一数据类型（如 Array）的实例，请重点关注一下是判断一个对象是否是数据类型的实例。在这里字面量值，2， true ，'str'不是实例，所以判断值为 false。

### constructor

```javascript
console.log((2).constructor === Number) // true
console.log(true.constructor === Boolean) // true
console.log('str'.constructor === String) // true
console.log([].constructor === Array) // true
console.log(function() {}.constructor === Function) // true
console.log({}.constructor === Object) // true
```

### Object.prototype.toString.call()

使用 Object 对象的原型方法 toString ，使用 call 进行狸猫换太子，借用 Object 的 toString 方法,不了解的可以去看一下 call 和 apply 的使用 方式

```javascript
var a = Object.prototype.toString

console.log(a.call(2)) //[object Number]
console.log(a.call(true)) //[object Boolean]
console.log(a.call('str')) //[object String]
console.log(a.call([])) //[object Array]
console.log(a.call(function() {})) //[object Function]
console.log(a.call({})) //[object Object]
console.log(a.call(undefined)) //[object undefined]
console.log(a.call(null)) // [object null]
```
