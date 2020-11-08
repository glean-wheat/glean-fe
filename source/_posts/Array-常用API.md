---
title: js 基础类型Array的常用方法
author: 张阳
tags:
  - JavaScript
  - Array
---

## Array 对象上常用的方法

### 不改变原数组的方法

#### 1.concat()

**语法：** array.concat(array1,array2,...,arrayN)

**功能：** 连接两个或多个数组

### 例子：

```js
var arr = [1, 0, 2, 4]
var arr2 = arr.concat(9, 9, 6)
console.log(arr) // [1, 0, 2, 4]
console.log(arr2) // [1, 0, 2, 4, 9, 9, 6]
```

#### 2.slice()

**语法：**array.slice(start,end)

**功能：**截取所需要的字符串

### 例子：

```js
var arr = [1, 0, 2, 4]
var arr2 = arr.slice(1, 2)
console.log(arr) // [1, 0, 2, 4]
console.log(arr2) // [0]
```

#### 3.join()

**语法：**array.join('-')

**功能：**合并所有字符串为一个字符串

## 例子：

```js
var arr = [1, 0, 2, 4]
var arr2 = arr.join('-')
console.log(arr) // [1, 0, 2, 4]
console.log(arr2) // 1-0-2-4
```

#### 4.toString()

语法：array.toString()

功能：把数组转为字符串

例子：

```js
var arr = [1, 0, 2, 4]
var arr2 = arr.join('-')
console.log(arr) // [1, 0, 2, 4]
console.log(arr2) // "1,0,2,4"
```

### 改变原有数组方法

### 1.fill()

语法：array.fill(value,start,end)

参数：
| 参数 | 描述 |
| :--- | :---: |
| value| 必须，填充的值 |
| start| 可选，开始填充的位置 |
| end | 可选，结束填充位置（默认为 array.length） |

功能：向原数组填充值,并替换掉原有的值

例子：

```js
var arr = [1, 0, 2, 4]
arr.fill(9)
console.log(arr) // [9, 9, 9, 9]
var arr2 = [1, 0, 2, 4]
arr2.fill(9, 2)
console.log(arr) // [1, 0, 9, 9]
arr2.fill(9, 1, 2)
console.log(arr) // [1, 0, 9, 4]
```

### 2.pop()

语法：array.pop()

功能：删除并返回数组的最后一个元素，如果数组为空，返回 undefined

例子：

```js
var arr = [1, 0, 2, 4]
var returnValue = arr.pop()
console.log(arr) //[1, 0, 2]
console.log(returnValue) //4
```

### 3.push()

语法：array.push()

功能：向数组末尾添加一个或多个元素

例子：

```js
var arr = [1, 0, 2, 4]
arr.push(9)
var arr2 = [1, 0, 2, 4]
arr2.push(9, 9, 6)
console.log(arr) //[1, 0, 2, 4, 9]
console.log(arr2) //[1, 0, 2, 4, 9, 9, 6]
```

### 4.shift()

语法：array.shift()

功能：删除数组第一个元素，并返回

例子：

```js
var arr = [1, 0, 2, 4]
var returnValue = arr.shift()
console.log(arr) //[0, 2, 4]
console.log(returnValue) //[1]
```

### 5.unshift()

语法：array.unshift(newVal1,newVal2,newVal3,...,newValN)

功能：向数组开始位置添加一个或者多个元素

参数：
|参数|描述|
|:---:|:---:|
|newVal1|必须，向数组添加的第一个元素|
|newVal2|可选，向数组添加的第二个元素|
|newValN|可选，向数组添加的第 N 个元素|

例子：

```js
var arr = [1, 0, 2, 4]
arr.unshift(9, 9, 6)
console.log(arr) //[9, 9, 6, 1, 0, 2, 4]
```

### 6.splice()

语法：array.splice(index,howmany,newVal1,...,newValN)

功能：向数组开始位置添加一个或者多个元素

参数：
|参数|描述|
|:---:|:---:|
|index|必须，删除/添加的位置，负数则为从末尾处开始计数|
|howmany|必须，删除项目数量，0 为不删除|
|newVal1,...,newValN|可选，向数组添 N 个新元素|

例子：

```js
var arr = [1, 0, 2, 4]
arr.splice(1, 2)
var arr2 = [1, 0, 2, 4]
arr2.splice(1, 2, 9, 9, 6)
console.log(arr) //[1, 4]
console.log(arr2) //[1, 9, 9, 6, 4]
```

### 7.reverse()

语法：array.reverse()

功能：翻转数组

例子：

```js
var arr = [1, 0, 2, 4]
arr.reverse()
console.log(arr) //[4, 2, 0, 1]
```

### 8.sort()

语法：array.sort(sortby)

参数：
|参数|描述|
|:---:|:---:|
|sortby|可选，函数|

说明：

- 如果不填参数，需要转为字符串，按照字符编码自动排序

- 参数为函数

  ```js
  function sortNumber(a, b) {
    return a - b
  }
  ```

  a < b 则 a 在 b 前面，a - b < 0 为升序

  a = b 不变

  a < b 则 a 在 b 后面 a - b > 0 为降序升序

例子：

```js
var arr = [1, 3, 2, 6, 9, 5]
arr.sort(function(a, b) {
  return a - b
})
var arr2 = [1, 3, 2, 6, 9, 5]
arr2.sort(function(a, b) {
  return b - a
})
console.log(arr) //[1, 2, 3, 5, 6, 9]
console.log(arr2) // [9, 6, 5, 3, 2, 1]
```
