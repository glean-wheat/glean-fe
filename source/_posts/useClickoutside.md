---
title: useClickoutside
author: GleanWu
tags:
  - react
  - hooks
---

> 小伙伴写了 vue 的 clickOutSide，我也来个 React Hooks 版，正好项目中用的到直接上代码

```javascript
import { useEffect, useCallback, useRef } from 'react'

const useClickOutside = (onClickOutside, dom, eventName = 'click', attachEle) => {
  const element = useRef('')
  const handleOutside = useCallback(
    (e) => {
      const targetElement = typeof dom === 'function' ? dom() : dom
      const el = targetElement || element.current
      if (el) {
        if (attachEle) {
          !(attachEle.contains(e.target) || el.contains(e.target)) && onClickOutside(e)
        } else {
          !el.contains(e.target) && onClickOutside(e)
        }
      }
    },
    [onClickOutside, dom, element]
  )
  useEffect(() => {
    // 使用事件捕获
    document.addEventListener(eventName, handleOutside, true)
    return () => {
      document.removeEventListener(eventName, handleOutside, true)
    }
  }, [eventName, onClickOutside, element])
  return element
}
export default useClickOutside
```

### 使用方式

```javascript
// useClickOutside使用介绍
const Demo = () => {
  const domRef = useClickOutside((e) => {
    console.log('点击了主要按钮外面', e)
  })
  useClickOutside((e) => {
    console.log('点击了普通按钮外', e)
  }, document.querySelector('#testbtn'))
  return (
    <div>
      <div ref={domRef}>
        <Button type="primary">主要按钮</Button>
      </div>
      <div id="testbtn">
        <Button type="line">普通按钮</Button>
      </div>
    </div>
  )
}
export default Demo
```

[Vue 版本](https://github.com/always-on-the-road/one-question-per-day/issues/63)
