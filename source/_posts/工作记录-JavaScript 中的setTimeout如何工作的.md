---
title: JavaScript 中的setTimeout如何工作的
author: GleanWu
tags:
  - 工作记录
  - setTimeout
---

关于 setTimeout 与 for 循环的相关面试问题
[setTimeout](https://juejin.im/post/6844903470466629640)

```javascript
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(new Date(), i)
  }, 1000)
}

console.log(new Date(), i)
```

关于上面的这个面试题，大家可以思考一下，会有两个情况，

- 情况 1：或说每隔 1s 输出一个 5
- 情况 2：会间隔 1s 后同时输出 5 个 5；
  当你有想法出现情况 1 的时候；说明就是对 setTimeout 的执行理解不够深入,推荐一篇文章[超级好文](https://johnresig.com/blog/how-javascript-timers-work/),里面讲到了 js 中时间的执行过程，

### 做一个简单的总结

浏览器的页面是通过消息队列和事件循环系统来驱动的。settimeout 的函数会被加入到延迟消息队列中，
等到执行完 Task 任务之后就会执行延迟队列中的任务。然后分析几种场景下面的 setimeout 的执行方式。

1. 如果执行一个很耗时的任务，会影响延迟消息队列中任务的执行
2. 存在嵌套带调用时候，系统会设置最短时间间隔为 4s（超过 5 层）
3. 未激活的页面，setTimeout 最小时间间隔为 1000ms
4. 延时执行时间的最大值 2147483647，溢出会导致定时器立即执行
5. setTimeout 设置回调函数 this 会是回调时候对应的 this 对象，可以使用箭头函数解决
