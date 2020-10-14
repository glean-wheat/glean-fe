---
title: 通过Blob实现前端下载文件
author: GleanWu
tags:
  - Blob
---

> 在开发中遇到遇到将用户选取的节点区域，生成截图，并上传
> 实现起来比较容易，我在此分享给大家

首先大家先要认识一下 `html2canvas` 这个库；[文档地址](http://html2canvas.hertzen.com/)，整体使用起来是比较简单的
也可以继续深入研究，这个库的社区还是很活跃的。

### Blob

另外大家需要简单了解一下 Blob,一直以来，JS 都没有比较好的可以直接处理二进制的方法。而 Blob 的存在，允许我们可以通过 JS 直接操作二进制数据。

    一个Blob对象就是一个包含有只读原始数据的类文件对象。Blob对象中的数据并不一定得是JavaScript中的原生形式。File接口基于Blob，继承了Blob的功能,并且扩展支持了用户计算机上的本地文件。

Blob 对象可以看做是存放二进制数据的容器，此外还可以通过 Blob 设置二进制数据的 MIME 类型

[MDN 文档地址](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)

### 代码

`npm i html2canvas --save`
`import html2canvas from 'html2canvas'`

```javascript
const box = document.querySelector('节点ID')
html2canvas(box).then(canvas => {
  const base64Img = canvas.toDataURL()
  // 通过生成的base64 图片，将其转成Blob
  const filesImg = base64ToBlob(base64Img)
  uploadImg(filesImg)
})

const base64ToBlob = code => {
  let parts = code.split(';base64,')
  let contentType = parts[0].split(':')[1]
  let raw = window.atob(parts[1])
  let rawLength = raw.length

  let uInt8Array = new Uint8Array(rawLength)

  for (let i = 0; i < rawLength; ++i) {
    uInt8Array[i] = raw.charCodeAt(i)
  }
  return new Blob([uInt8Array], { type: contentType })
}

const uploadImg = filesImg => {
  let param = new FormData()
  param.append('file', filesImg, updataTypes)
  // 调用自己的上传  这样就行了
  axios('/upload', {
    type: 'upload',
    name: 'file', // 文件参数
    data: param,
    file: filesImg, // 文件x/
    params: {
      name: formValue.blockName
    }, // 其他参数
    withCredentials: true,
    onUploadProgress: event => {}
  }).then(res => {
    console.log('上传结果', res)
    // 上传进度
  })
}
```
