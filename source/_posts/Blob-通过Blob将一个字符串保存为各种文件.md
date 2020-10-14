---
title: 通过Blob将一个字符串保存为各种文件
author: GleanWu
tags:
  - Blob
---

结合 [#78](https://github.com/zerolab-fe/one-question-per-day/issues/78)文章中讲到了将服务端给的二进制文件直接进行保存；二进制文件中包含了文件类型和内容；如果实现纯前端保存一段字符串为.txt、.js、css 文件呢。其他的文件类型都是可以的；
自己封装了一个方法，

```javascript
const downloadFileByBlob = (blobUrl, filename) => {
  const eleLink = document.createElement('a')
  eleLink.download = filename
  eleLink.style.display = 'none'
  eleLink.href = blobUrl
  // 触发点击
  document.body.appendChild(eleLink)
  eleLink.click()
  // 然后移除
  document.body.removeChild(eleLink)
}

const downFile = (config, type, name) => {
  const blobContent = new Blob([config], {
    type: type
  })
  const blobUrl = window.URL.createObjectURL(blobContent)
  downloadFileByBlob(blobUrl, name)
  // 需要注意
  window.URL.revokeObjectURL(blobUrl)
}

const downFiles = () => {
  const { jsCode, cssCode } = {
    title: '文件名称',
    jsCode: `const test ='hello word'`,
    cssCode: `.test{
                width:100px 
         }`
  }

  downFile(jsCode, 'text/javascript', title + '.jsx') // 根据文件类型；传入一个合适的 MIME 类型
  downFile(cssCode, 'text/css', title + '.scss')
}
```

### 注意

在每次调用 createObjectURL() 方法时，都会创建一个新的 URL 对象，即使你已经用相同的对象作为参数创建过。当不再需要这些 URL 对象时，每个对象必须通过调用 URL.revokeObjectURL() 方法来释放。

浏览器在 document 卸载的时候，会自动释放它们，但是为了获得最佳性能和内存使用状况，你应该在安全的时机主动释放掉它们。

### 其他资料

[MIME](https://www.w3school.com.cn/media/media_mimeref.asp)
