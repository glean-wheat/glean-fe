---
title: postMessage知识普及
author: GleanWu
tags:
  - postMessage
  - javascript
---

在开发中遇到一个问题，就是 iframe 怎么和父级页面通讯呢；

一般通过父调用子页面的函数是没问题的；但是如果子页面想要调用父页面的函数的话就会报

```javascript
Uncaught DOMException: Blocked a frame with origin "null" from accessing a cross-origin frame
```

就是跨域调用限制；
通过一番寻找，想到了 postMessage 方法

### postMessage

**window.postMessage()** 方法可以安全地实现跨源通信。通常，对于两个不同页面的脚本，只有当执行它们的页面位于具有相同的协议（通常为 https），端口号（443 为 https 的默认值），以及主机 (两个页面的模数 **Document.domain**设置为相同的值) 时，这两个脚本才能相互通信。**window.postMessage()** 方法提供了一种受控机制来规避此限制，只要正确的使用，这种方法就很安全。

从广义上讲，一个窗口可以获得对另一个窗口的引用（比如 **targetWindow = window.opener**），然后在窗口上调用 **targetWindow.postMessage()** 方法分发一个 MessageEvent 消息。接收消息的窗口可以根据需要自由处理此事件。传递给 **window.postMessage()** 的参数（比如 message ）**将通过消息事件对象暴露给接收消息的窗口**。

#### 语法如下：

> otherWindow.postMessage(message, targetOrigin, [transfer]);

#### 父页面

```html
<body>
  <p>我是父页面</p>
  <p>接收到的iframe信息是：<span id="content"></span></p>
  <iframe src="/btoa/b.html" frameborder="0" marginheight="0" marginwidth="0"></iframe>
  <script>
    // event 参数中有 data 属性，就是iframe页面发送过来的数据
    window.addEventListener(
      'message',
      function(event) {
        // 把iframe页面发送过来的数据显示在父页面中
        document.getElementById('content').innerHTML = event.data
      },
      false
    )
  </script>
</body>
```

#### iframe

```html
<body>
  <p>我是iframe页面</p>
  <div>发送的信息是：<input id="iframe" type="text" /><input id="sendBtn" type="button" value="发送" /></div>
  <script>
    // 点击按钮后向父页面发送数据
    document.getElementById('sendBtn').onclick = function() {
      // window.parent代表父页面
      window.parent.postMessage(document.getElementById('iframe').value, '*')
    }
  </script>
</body>
```

#### 兼容性

![image](https://user-images.githubusercontent.com/24740506/91671875-e7ff2c80-eb5c-11ea-8d0b-4ca2bbb07cac.png)
