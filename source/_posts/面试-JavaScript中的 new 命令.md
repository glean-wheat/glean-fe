---
title: JavaScript中的 new 命令
author: GleanWu
tags:
  - 面试
  - new
---

### [定义](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)

**new 运算符** 创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例

### 描述

new 关键字会进行如下的操作

1. 创建一个空的 js 对象 即（{}）;
2. 链接该对象（即设置该对象的构造函数）到另一个对象 ;
3. 将步骤 1 创建的对象作为 this 的上下文
4. 如果该函数没有返回对象则返回 this

### 手写实现

> 这段代码引用了《Javascript 设计模式与开发实践》，根据自己的理解添加了一些注释

```javascript
function Person(name) {
  this.name = name
}
Person.prototype.getName = function() {
  return this.name
}

var ObjectFactory = function() {
  // 创建一个对象
  var obj = new Object()
  // 返回第一个参数作为构造函数
  var Constructor = [].shift.call(arguments)
  // 将构造函数的原型复制于对象的原型
  obj.__proto__ = Constructor.prototype
  // 调用构造函数，并将obj 作为this, arguments作为参数
  var ret = Constructor.apply(obj, arguments)
  // 如果构造函数里返回一个对象的话，就直接返回，否则我们就返回this即new创建的对象
  return typeof ret === 'object' ? ret : obj
}
// 效果等效
var a = ObjectFactory(Person, 'sven')
console.log(Object.getPrototypeOf(a) === Person.prototype)
```
