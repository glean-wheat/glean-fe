---
title: MutationObserver 属性
author: GleanWu
tags:
  - javascript
  - html
---

> 在做水印防止篡改功能的时候，MutationObserver 就是一个很有必要的属性了

MutationObserver 接口提供了监视对 DOM 树所做更改的能力。它被设计为旧的 Mutation Events 功能的替代品，该功能是 DOM3 Events 规范的一部分。

```javascript
// 选择需要观察变动的节点
const targetNode = document.getElementById('some-id')

// 观察器的配置（需要观察什么变动）
const config = { attributes: true, childList: true, subtree: true }

// 当观察到变动时执行的回调函数
const callback = function(mutationsList, observer) {
  // Use traditional 'for loops' for IE 11
  for (let mutation of mutationsList) {
    if (mutation.type === 'childList') {
      console.log('A child node has been added or removed.')
    } else if (mutation.type === 'attributes') {
      console.log('The ' + mutation.attributeName + ' attribute was modified.')
    }
  }
}

// 创建一个观察器实例并传入回调函数
const observer = new MutationObserver(callback)

// 以上述配置开始观察目标节点
observer.observe(targetNode, config)

// 之后，可停止观察
observer.disconnect()
```

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)
