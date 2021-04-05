---
title: javascript 一句话算农历
author: GleanWu
tags:
  - javascript
---

# 太强了！！！强烈推荐一波

```javascript
// 就是这一句
new Date().toLocaleString('zh-CN-u-ca-chinese')
```

```javascript
// 年
new Date()
  .toLocaleString('zh-CN-u-ca-chinese')
  .replace(
    /(\d+)\s*?年/,
    (_, y) => '甲乙丙丁戊己庚辛壬癸'.charAt((y - 4) % 10) + '子丑寅卯辰巳午未申酉戌亥'.charAt((y - 4) % 12)
  )
```
