---
title: 关于preventDefault 一个小的发现
author: GleanWu
tags:
  - 工作记录
  - preventDefault
---

直接上代码

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
    <div>
      <ul>
        <li id="wrapper">
          <form>
            <input onfocus="inputFocus()" />
          </form>
          <div>
            <a href="http://www.xiaomi.com">这是一个连接</a>
          </div>
        </li>
        <li>
          <a href="http://www.xiaomi.com">这是一个连接2</a>
        </li>
      </ul>
    </div>
  </body>
  <script>
    const el = document.querySelector('#wrapper')
    el.addEventListener('click', (e) => {
      console.log(e.target)
      if (e) {
        e.preventDefault()
        console.log('阻止默认事件')
      }
    })
    function inputFocus(e) {
      console.log('获取焦点')
    }
  </script>
</html>
```

![image](https://user-images.githubusercontent.com/24740506/90085682-28803d00-dd4b-11ea-8e69-e8862df406bc.png)

当你点击第一个链接的时候 是不会跳转的；因为它的父级有个 e.preventDefault() 同时也把它所有的子节点默认事件都禁止了
