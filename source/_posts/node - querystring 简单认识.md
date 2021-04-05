---
title: node - querystring 简单认识
author: GleanWu
tags:
  - node
---

今日新学的知识点；以前应该也在用这个功能模块，估计是没关注，在开发中因为对它的不熟悉导出开发出现奇怪的问题；本篇文章先简单的介绍一下它的基础 API，接下来在介绍一下 Content-Type;才能完整的描述我的心路历程，先给大家看一下我进行下午遇到的问题截图

![占位图需要换掉](https://user-images.githubusercontent.com/24740506/96462856-a8011f80-1258-11eb-83d4-16a6d1d452e1.png)
![requestBody](https://user-images.githubusercontent.com/24740506/96463131-f6aeb980-1258-11eb-9506-acc1edc60a8e.png)

第一个图里面的最后一个`{}`包裹的 是我打印的 request.body,能看到里面打印的字符串根本不是我想要的；接下来大家先了解一下 querystring 下一篇中会再介绍 Content-Type；弥补一下基础知识的不足

**_千万要和 JSON.parse 已经 JOSN.stringify，自己原来是认为和 querystring 中的相同 API 转化出的来的字符串是一样的呢，实际上是不一样的_**

> querystring 模块提供用于解析和格式化 URL 查询字符串的实用工具。 可以使用以下方式访问它

    const querystring = require('querystring');

# 重点关注

## querystring.parse(str[, sep[, eq[, options]]])

- str <string> 要解析的 URL 查询字符串。
- sep <string> 用于在查询字符串中分隔键值对的子字符串。**默认值: '&'**。
- eq <string> 用于在查询字符串中分隔键和值的子字符串。**默认值: '='**。
- options <Object> - `decodeURIComponent <Function>` 当解码查询字符串中的百分比编码字符时使用的函数。默认值:`querystring.unescape()`。
  `maxKeys <number>` 指定要解析的键的最大数量。指定 0 可移除键的计数限制。默认值: `1000`。
  querystring.parse() 方法将 URL 查询字符串 str 解析为键值对的集合。

例如，查询字符串`'foo=bar&abc=xyz&abc=123'` 会被解析为：

```
{
  foo: 'bar',
  abc: ['xyz', '123']
}
```

`querystring.parse()` 方法返回的对象不是原型地继承自 `JavaScript` 的 `Object`。 这意味着典型的 `Object` 方法如 `obj.toString()`、 `obj.hasOwnProperty()`等都没有被定义并且不起作用。

默认情况下，会假定查询字符串中的百分比编码字符使用 UTF-8 编码。 如果使用其他的字符编码，则需要指定其他的 decodeURIComponent 选项：

```
// 假设 gbkDecodeURIComponent 函数已存在。

querystring.parse('w=%D6%D0%CE%C4&foo=bar', null, null,
                  { decodeURIComponent: gbkDecodeURIComponent });
```

## querystring.stringify(obj[, sep[, eq[, options]]])

- `obj <Object>` 要序列化为 URL 查询字符串的对象。 -`sep <string>` 用于在查询字符串中分隔键值对的子字符串。默认值: '&'。
- `eq <string>` 用于在查询字符串中分隔键和值的子字符串。默认值: '='。
- options - `encodeURIComponent <Function>` 当将查询字符串中不安全的 URL 字符转换为百分比编码时使用的函数。默认值: `querystring.escape()`。
  `querystring.stringify()` 方法通过遍历对象的自身属性从给定的 obj 生成 URL 查询字符串。

它会序列化传入的 obj 中以下类型的值：`<string> | <number> | <boolean> | <string[]> | <number[]> | <boolean[]>`。 任何其他的输入值都将会被强制转换为空字符串。

```
querystring.stringify({ foo: 'bar', baz: ['qux', 'quux'], corge: '' });
// 返回 'foo=bar&baz=qux&baz=quux&corge='

querystring.stringify({ foo: 'bar', baz: 'qux' }, ';', ':');
// 返回 'foo:bar;baz:qux'
```

默认情况下，查询字符串中需要进行百分比编码的字符将会被编码为 `UTF-8`。 如果需要其他的编码，则需要指定其他的 `encodeURIComponent` 选项：

```
// 假设 gbkEncodeURIComponent 函数已存在。

querystring.stringify({ w: '中文', foo: 'bar' }, null, null,
                      { encodeURIComponent: gbkEncodeURIComponent });
```

# 其他

## querystring.decode()

`querystring.decode()` 函数是 `querystring.parse()` 的别名

## querystring.escape(str)

`querystring.escape()` 方法以对 URL 查询字符串的特定要求进行了优化的方式对给定的 str 执行 URL 百分比编码。

`querystring.escape()` 方法由 `querystring.stringify()` 使用，通常不会被直接地使用。 它的导出主要是为了允许应用程序代码在需要时通过将 `querystring.escape` 赋值给替代函数来提供替换的百分比编码实现。

# 推荐阅读

node 自带模块 [querystring 模块](http://nodejs.cn/api/querystring.html)

字符串分解库 [qs](https://www.npmjs.com/package/qs) 功能相对比较全
