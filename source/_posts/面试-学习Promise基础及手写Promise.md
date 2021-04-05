---
title: 学习Promise基础及手写Promise
author: GleanWu
tags:
  - 面试
  - promise
---

### 个人对 Promise 的理解

可以这么说，Promise 已经成为现代前端的"水"和"电"，很是关键
![image](https://user-images.githubusercontent.com/24740506/89840052-e071e680-dba1-11ea-9b40-a17cc3180235.png)

上图展示的是一个标准的异步编程模型，页面主线程发起了一个耗时的任务，并将任务交给另外一个进程去处理，这时页面主线程会继续执行消息队列中的任务。等该进程处理完这个任务后，会将该任务添加到渲染进程的消息队列中，并排队等待循环系统的处理。排队结束之后，循环系统会取出消息队列中的任务进行处理，并触发相关的回调操作。这就是页面编程的一大特点：异步回调。Web 页面的单线程架构决定了异步回调，而异步回调影响到了我们的编码方式，到底是如何影响的呢
接下来会继续和大家分享，什么是消息队列和事件循环，今天咱们先只谈 Promise,

```

//执行状态
function onResolve(response){console.log(response) }
function onReject(error){console.log(error) }

let xhr = new XMLHttpRequest()
xhr.ontimeout = function(e) { onReject(e)}
xhr.onerror = function(e) { onReject(e) }
xhr.onreadystatechange = function () { onResolve(xhr.response) }

//设置请求类型，请求URL，是否同步信息
let URL = 'https://www.***.com'
xhr.open('Get', URL, true);

//设置参数
xhr.timeout = 3000 //设置xhr请求的超时时间
xhr.responseType = "text" //设置响应返回的数据格式

//发出请求
xhr.send();
```

我们执行上面这段代码，可以正常输出结果的。但是，这短短的一段代码里面竟然出现了五次回调，这么多的回调会导致代码的逻辑不连贯、不线性，非常不符合人的直觉，这就是异步回调影响到我们的编码方式。那有什么方法可以解决这个问题吗？当然有，我们可以封装这堆凌乱的代码，降低处理异步回调的次数。然后接下来就会遇到的问题就是回调地狱；
这个时候大家自然会想到 Promise 它其实并没解决回调的问题只是进行了代码书写优化的一种方案；使得我们在写代码的时候能够以一个线性的思维是处理这个问题；

### 做了一个简单的铺垫后,接下来咱么了解一下 Promise

```javascript
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('foo')
  }, 300)
})

promise1.then((value) => {
  console.log(value)
  // expected output: "foo"
})

console.log(promise1)
// expected output: [object Promise]
```

**Promise**对象是一个代理对象（代理一个值），被代理的值在**Promise**对象创建时可能是未知的。它允许你为异步操作的成功和失败分别绑定相应的处理方法（handlers）。 这让异步方法可以像同步方法那样返回值，但并不是立即返回最终执行结果，而是一个能代表未来出现的结果的 promise 对象

一个 Promise 有以下几种状态:

- pending: 初始状态，既不是成功，也不是失败状态。
- fulfilled: 意味着操作成功完成。
- rejected: 意味着操作失败。
  pending 状态的 Promise 对象可能会变为 fulfilled 状态并传递一个值给相应的状态处理方法，也可能变为失败状态（rejected）并传递失败信息。当其中任一种情况出现时，Promise 对象的 then 方法绑定的处理方法（handlers ）就会被调用（then 方法包含两个参数：onfulfilled 和 onrejected，它们都是 Function 类型。当 Promise 状态为 fulfilled 时，调用 then 的 onfulfilled 方法，当 Promise 状态为 rejected 时，调用 then 的 onrejected 方法， 所以在异步操作的完成和绑定处理方法之间不存在竞争）。

### 简单的手写 promise

```javascript
const PENDING = 'PENDING'
const RESOLVED = 'RESOLVED'
const REJECTED = 'REJECTED'

class MyPromise {
  constructor(executor) {
    this.status = PENDING // 宏变量, 默认是等待态
    this.value = undefined // then方法要访问到所以放到this上
    this.reason = undefined // then方法要访问到所以放到this上
    let resolve = (value) => {
      if (this.status === PENDING) {
        // 保证只有状态是等待态的时候才能更改状态
        this.value = value
        this.status = RESOLVED
      }
    }
    let reject = (reason) => {
      if (this.status === PENDING) {
        this.reason = reason
        this.status = REJECTED
      }
    }
    // 执行executor传入我们定义的成功和失败函数:把内部的resolve和reject传入executor中用户写的resolve, reject
    try {
      executor(resolve, reject)
    } catch (e) {
      console.log('catch错误', e)
      reject(e) //如果内部出错 直接将error手动调用reject向下传递
    }
  }
  then(onfulfilled, onrejected) {
    if (this.status === RESOLVED) {
      onfulfilled(this.value)
    }
    if (this.status === REJECTED) {
      onrejected(this.reason)
    }
  }
}
module.exports = MyPromise
```

想要完全了解 Promise，涉及到的内容有很多，我在这只是做到了一个很简单的总结，我现在还在研究微任务和宏认为，想要在这个角度对 Promise 也有一个原理的分析，敬请期待
