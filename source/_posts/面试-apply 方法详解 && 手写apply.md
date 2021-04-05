---
title: apply 方法详解 && 手写apply
author: GleanWu
tags:
  - 面试
  - call
  - apply
---

继续 call 的分析 分析完了应该对 apply 就清楚了

```javascript
//通过代码异步一步进行分析call

function ObjB() {
  this.message = 'messageB'
  this.setMessage = function(arg) {
    this.message = arg
  }
}

function ObjA() {
  this.message = 'messageA'
  this.getMessage = function() {
    return this.message
  }
}

var b = new ObjB()
var a = new ObjA()

//可以直接访问属性.输出为messageA
console.log(a.message)

//给对象ObjA动态指派ObjB的setMessage方法,注意,ObjA本身是没有这方法的!
b.setMessage.call(a, 'A的消息')
//输出"A的消息"
console.log(a.getMessage())
/**
 * 给对象b动态指派a的getMessage方法,注意,b本身也是没有这方法的!
 * 可以理解为得到A里面的getMessage方法体.然后用call注入到B里面执行.
 * 因为上下文变成了B,所以B多了getMessage方法体.
 */
var result = a.getMessage.call(b)
//输出"messageB"
console.log(result)
/**
 * Uncaught TypeError: Object #<ObjB> has no method 'getMessage' 出错了.
 * 因为call并没有把getMessage永久注入进ObjB
 */
//log(b.getMessage());

// 通过手写myCall 打印进行分析

Function.prototype.myCall = function(context, ...arg) {
  const context = context || window
  const fn = Symbol('cb')
  context[fn] = this //此处this是指调用myCall的function
  const result = context[fn](...arg)
  console.log('context', context, this) // *** ///
  delete context[fn]
  return result
}

// 调用
b.setMessage.myCall(a, 'A的消息')

/**
  * myCall中console.log
  * context  === ObjA 
  * this === ƒ (arg) {  
        this.message = arg;  
    }
  */
```

其实看完上面的代码；大家应该就很清楚 call 的实现原理了；总结一句话
**改变函数的调用者**

先说一下 apply 和 call 的区别

**call, apply 方法区别是,从第二个参数起, call 方法参数将依次传递给借用的方法作参数, 而 apply 直接将这些参数放到一个数组中再传递, 最后借用方法的参数列表是一样的**
写法上的区别

```
print.call(this, a, b, c, d);
print.apply(this, [a, b, c, d]);
```

其实再去重写的就变得很简单了

### ES6 的

```javascript
Function.prototype.myApply = function(context, arg) {
  const context = context || window
  const fn = Symbol('cb')
  context[fn] = this //此处this是指调用myCall的function
  const result = context[fn](...arg)
  delete context[fn]
  return result
}
```

### 经典写法

```
Function.prototype.myApply = function(context, args) {    
    context = context ? Object(context) : window;    
    context.fn = this;   
     if (!args) { 
        return context.fn();    
    }    
    let res = eval('context.fn('+ args +')');    
    delete context.fn;   
    return res;
}

```
