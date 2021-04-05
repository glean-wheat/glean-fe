---
title: call 方法详解 && 手写call
author: GleanWu
tags:
  - 面试
  - call
  - apply
---

在 javascript 中，**call** 和 **apply** 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 **this** 的指向。
JavaScript 的一大特点是，函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。

### call 语法

```

fun.call(thisArg, arg1, arg2, ...)
thisArg:：在fun函数运行时指定的this值。需要注意的是，指定的this值并不一定是该函数执行时真正的this值，
如果这个函数处于非严格模式下，则指定为null和undefined的this值会自动指向全局对象(浏览器中就是window对象)，
同时值为原始值(数字，字符串，布尔值)的this会指向该原始值的自动包装对象。
arg1, arg2, ... 指定的参数列表。
```

### ES6 的简单写法

```javascript
Function.prototype.myCall = function(context, ...arg) {
  const context = context || window
  const fn = Symbol('cb')
  context[fn] = this //此处this是指调用myCall的function
  const result = context[fn](...arg)
  delete context[fn]
  return result
}
```

### ES5 经典写法

```javascript
/**
 * 每个函数都可以调用call方法，来改变当前这个函数执行的this关键字，并且支持传入参数
 */
Function.prototype.myCall = function(context) {
  //第一个参数为调用call方法的函数中的this指向
  var context = context || window
  //将this赋给context的fn属性
  context.fn = this //此处this是指调用myCall的function

  var arr = []
  for (var i = 0, len = arguments.length; i < len; i++) {
    arr.push('arguments[' + i + ']')
  }
  //执行这个函数，并返回结果
  var result = eval('context.fn(' + arr.toString() + ')')
  //将this指向销毁
  delete context.fn
  return result
}
```
