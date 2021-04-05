---
title: ES6 新语法（一）
author: GleanWu
tags:
  - JavaScript
  - HTML
---

> 今天在编写组件的时候遇到了一个问题，就是要求输入框的高度随着内容自动变高；textarea 本身肯定是不支持的；立刻就想到了 contenteditable 属性

```html
开头安利一个小技巧 在地址栏输入 data:text/html,
<html contenteditable>
  这样浏览器就变成了编辑器
</html>
```

### 关于 HTML 部分

```html
<div contenteditable id="editable">这里面的内容是可以编辑的</div>
```

## script 部分

```javascript
const editorWrapper = document.querySelector('#editable')
editorWrapper.onblur = (e) => {
  console.log('失去焦点')
  console.dir(e)
}

editorWrapper.onfocus = (e) => {
  console.log('获取焦点')
  console.dir(e)
}

editorWrapper.oninput = (e) => {
  console.log('模拟change事件')
  console.dir(e.target.innerHTML)
}
```

### 支持以下事件;

- 当失去焦点时; **editable.onblur=function(e){}** 事件
- 当获取焦点时; **editable.onfocus=function(e){}** 事件
- 使用 oninput 来的代替 onChange 事件
  **但是常用的 change 事件是不支持的;**

### 全部代码

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div contenteditable id="editable">这里面的内容是可以编辑的</div>
  </body>
  <script>
    const editorWrapper = document.querySelector('#editable')
    editorWrapper.onblur = (e) => {
      console.log('失去焦点')
      console.dir(e)
    }

    editorWrapper.onfocus = (e) => {
      console.log('获取焦点')
      console.dir(e)
    }

    editorWrapper.oninput = (e) => {
      console.log('模拟change事件')
      console.dir(e.target.innerHTML)
    }
  </script>
</html>
```
