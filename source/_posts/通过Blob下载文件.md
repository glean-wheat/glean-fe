---
title: 通过Blob实现前端下载文件
author: GleanWu
tags:
  - Blob
---

> 在封装请求库的时候，想要把 download 方法也集成到自己的请求库中，自己想了一下下载的方式进行一下记录

### Blob

先撸代码吧

```javascript
import axios from 'axios'

const download = (options, host) => {
  const { filename = '未命名' } = options
  const url = host ? host + options.url : options.url

  // 设置类型，防止出现乱码
  Object.assign(options, { responseType: 'blob' })
  axios({ ...options, url }).then(
    (res) => {
      const { downloadSuccess } = options
      const blob = new window.Blob([res.data])
      const downloadElement = document.createElement('a')
      const href = window.URL.createObjectURL(blob) // 创建下载的链接
      downloadElement.href = href
      downloadElement.download = filename // 下载后文件名
      document.body.appendChild(downloadElement)
      downloadElement.click() // 点击下载
      document.body.removeChild(downloadElement) // 下载完成移除元素
      window.URL.revokeObjectURL(href) // 释放blob对象
      downloadSuccess && downloadSuccess(res)
    },
    (error) => {
      const { downloadFail } = options
      downloadFail && downloadFail(error)
    }
  )
}
export default download
```

#### 参考链接

- https://developer.mozilla.org/zh-CN/docs/Web/API/Blob
- https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL
- https://developer.mozilla.org/zh-CN/docs/Web/API/File/Using_files_from_web_applications
- https://developer.mozilla.org/zh-CN/docs/Web/API/URL/revokeObjectURL

### window.open

这个方法也是可以在直接下载二进制的文件的，这个就不详细介绍了
