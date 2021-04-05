---
title: hash 与 history 的区别
author: GleanWu
tags:
  - 路由
---

SPA 需要在不刷新页面的情况下做页面更新的能力，这就需要引入前端路由，实际上，前端路由是利用了浏览器的 hash 或 history 属性。

### hash 模式

这里的 hash 就是指 url 尾巴后的 # 号以及后面的字符。这里的 # 和 css 里的 # 是一个意思。hash 也 称作 **锚点**，本身是用来做页面定位的，她可以使对应 id 的元素显示在可视区域内。

由于 hash 值变化不会导致浏览器向服务器发出请求，而且 hash 改变会触发 hashchange 事件，浏览器的进后退也能对其进行控制，所以人们在 html5 的 history 出现前，基本都是使用 hash 来实现前端路由的。

### API

```javascript
window.location.hash = 'qq' // 设置 url 的 hash，会在当前url后加上 '#qq'

var hash = window.location.hash // '#qq'

window.addEventListener('hashchange', function() {
  // 监听hash变化，点击浏览器的前进后退会触发
})
```

### history

已经有 hash 模式了，而且 hash 能兼容到 IE8， history 只能兼容到 IE10，为什么还要搞个 history 呢？
首先，hash 本来是拿来做页面定位的，如果拿来做路由的话，原来的锚点功能就不能用了。其次，hash 的传参是基于 url 的，如果要传递复杂的数据，会有体积的限制，而 history 模式不仅可以在 url 里放参数，还可以将数据存放在一个特定的对象中。
最重要的一点：想要让地址栏里面更加美观

### [API](https://developer.mozilla.org/zh-CN/docs/Web/API/History)

```javascript
window.history.pushState(state, title, url)
// state：需要保存的数据，这个数据在触发popstate事件时，可以在event.state里获取
// title：标题，基本没用，一般传 null
// url：设定新的历史记录的 url。新的 url 与当前 url 的 origin 必须是一樣的，否则会抛出错误。url可以是绝对路径，也可以是相对路径。
//如 当前url是 https://www.baidu.com/a/,执行history.pushState(null, null, './qq/')，则变成 https://www.baidu.com/a/qq/，
//执行history.pushState(null, null, '/qq/')，则变成 https://www.baidu.com/qq/

window.history.replaceState(state, title, url)
// 与 pushState 基本相同，但她是修改当前历史记录，而 pushState 是创建新的历史记录

window.addEventListener('popstate', function() {
  // 监听浏览器前进后退事件，pushState 与 replaceState 方法不会触发
})

window.history.back() // 后退
window.history.forward() // 前进
window.history.go(1) // 前进一步，-2为后退两步，window.history.lengthk可以查看当前历史堆栈中页面的数量
```
