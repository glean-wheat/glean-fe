---
title: apply 方法详解 && 手写apply
author: GleanWu
tags:
  - 面试
  - call
  - apply
  - bind
---

bind 跟 apply,call 的本质区别，bind 不会改变原函数的 this 指向，只会返回一个新的函数（我们想要的那个 this 指向），并且不会调用。但是 apply 和 call 会改变原函数的 this 指向并且直接调用

```javascript
Function.prototype.myBind = function(objThis, ...params) {
  const thisFn = this // 存储源函数以及上方的params(函数参数)
  // 对返回的函数 secondParams 二次传参
  let fToBind = function(...secondParams) {
    console.log('secondParams', secondParams, ...secondParams)
    const isNew = this instanceof fToBind // this是否是fToBind的实例 也就是返回的fToBind是否通过new调用
    const context = isNew ? this : Object(objThis) // new调用就绑定到this上,否则就绑定到传入的objThis上
    return thisFn.call(context, ...params, ...secondParams) // 用call调用源函数绑定this的指向并传递参数,返回执行结果
  }
  fToBind.prototype = Object.create(thisFn.prototype) // 复制源函数的prototype给fToBind
  return fToBind // 返回拷贝的函数
}
```
