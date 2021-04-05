---
title: JSON.stringify()
author: GleanWu
tags:
  - JSON.stringify
  - javascript
---

> 关联集合 [AST 研究中](https://github.com/always-on-the-road/one-question-per-day/issues/51)

## 什么是 JSON.stringify() ；

JSON.stringify() 方法将一个 JavaScript 值（对象或者数组）转换为一个 JSON 字符串，如果指定了 replacer 是一个函数，则可以选择性地替换值，或者如果指定了 replacer 是一个数组，则可选择性地仅包含数组指定的属性。

## 语法

> JSON.stringify(value[, replacer [, space]])

**语法**

##### value

将要序列化成 一个 JSON 字符串的值。

##### replacer 可选

如果该参数是一个函数，则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理；如果该参数是一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中；如果该参数为 null 或者未提供，则对象所有的属性都会被序列化；关于该参数更详细的解释和示例

###### space 可选

指定缩进用的空白字符串，用于美化输出（pretty-print）；如果参数是个数字，它代表有多少的空格；上限为 10。该值若小于 1，则意味着没有空格；如果该参数为字符串（当字符串长度超过 10 个字母，取其前 10 个字母），该字符串将被作为空格；如果该参数没有提供（或者为 null），将没有空格。

##### JSON.stringify() 将值转换为相应的 JSON 格式：

- 转换值如果有 toJSON() 方法，该方法定义什么值将被序列化。
- 非数组对象的属性不能保证以特定的顺序出现在序列化后的字符串中。
- 布尔值、数字、字符串的包装对象在序列化过程中会自动转换成对应的原始值。
  undefined、任意的函数以及 symbol 值，在序列化过程中会被忽略（出现在非数组对象的属性值中时）或者被转换成 null（出现在数组中时）。

- 函数、undefined 被单独转换时，会返回 undefined，如 JSON.stringify(function(){}) or JSON.stringify(undefined).
- 对包含循环引用的对象（对象之间相互引用，形成无限循环）执行此方法，会抛出错误。
- 所有以 symbol 为属性键的属性都会被完全忽略掉，即便 replacer 参数中强制指定包含了它们。
- Date 日期调用了 toJSON() 将其转换为了 string 字符串（同 Date.toISOString()），因此会被当做字符串处理。
- NaN 和 Infinity 格式的数值及 null 都会被当做 null。
  其他类型的对象，包括 Map/Set/WeakMap/WeakSet，仅会序列化可枚举的属性。

**注意:** 不能用 replacer 方法，从数组中移除值（values），如若返回 undefined 或者一个函数，将会被 null 取代。

#### 重点是 replacer 参数

#### demo1 (function)

```
function replacer(key, value) {
  if (typeof value === "string") {
    return undefined;
  }
  return value;
}

var foo = {
    foundation: "Mozilla",
    model: "box",
    week: 45,
    transport: "car",
    month: 7
};
var jsonString = JSON.stringify(foo, replacer);

// 输出结果 {"week":45,"month":7}.
```

#### demo2 (array)

如果 replacer 是一个数组，数组的值代表将被序列化成 JSON 字符串的属性名。

```
JSON.stringify(foo, ['week', 'month']);
// '{"week":45,"month":7}', 只保留 "week" 和 "month" 属性值。
```

#### space 参数

space 参数用来控制结果字符串里面的间距。如果是一个数字, 则在字符串化时每一级别会比上一级别缩进多这个数字值的空格（最多 10 个空格）；如果是一个字符串，则每一级别会比上一级别多缩进该字符串（或该字符串的前 10 个字符）。

```
JSON.stringify({ a: 2 }, null, " ");   // '{\n "a": 2\n}'
JSON.stringify({ uno: 1, dos : 2 }, null, '\t')
// '{            \
//     "uno": 1, \
//     "dos": 2  \
// }'

```

今天写这个的目的是；在做 node 开发的时候；使用 cosole.log 打印对象的时候；拿到的是[Object];这个时候就可以使用
不用的时候

![AST抽象语法树](https://user-gold-cdn.xitu.io/2020/4/8/1715a25fda2c0453?w=886&h=992&f=png&s=238750)

```
使用  console.log(JSON.stringify(add, null, 4))
```

就是这个样子：

![AST抽象语法树](https://user-gold-cdn.xitu.io/2020/4/8/1715a255ab14c467?w=1194&h=1590&f=png&s=372007)
【AST 抽象语法树】：正在研究怎么拆解；怎么加工合并
