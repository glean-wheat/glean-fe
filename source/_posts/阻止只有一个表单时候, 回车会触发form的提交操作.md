---
title: 阻止只有一个表单时候, 回车会触发form的提交操作
author: GleanWu
tags:
  - Javascript
---

### 方式 1

```javascript
// react
<form
  onSubmit={(e) => {
    // 阻止只有一个表单时候；回车会触发form的提交操作
    e.preventDefault()
    return false
  }}
>
  {children}
</form>
```

### 方式 2

```javascript
componentDidMount() {
    document.querySelectorAll('form').forEach(ele => {
      ele.addEventListener('submit', e => {
        // 阻止只有一个表单时候；回车会触发form的提交操作
        e.preventDefault()
        return false
      })
    })
  }
```
