---
title: AST抽象语法树
author: GleanWu
tags:
  - AST
---

> 在[上一篇](https://github.com/always-on-the-road/one-question-per-day/issues/50)中,自己在写插件过程中，需要写一个对 react-jsx 的解析器; 反向解出 Schema,通过反向序列化后，允许线上编辑器可以对该 Schema 进行解析并显示；所以就对 AST 语法又进行了学了，接下来进行一个简单的记录

### 关于百度百科定义

什么是抽象语法树(Abstract Syntax Tree ，AST)？

在计算机科学中，抽象语法树（Abstract Syntax Tree，AST），或简称语法树（Syntax tree），是源代码语法结构的一种抽象表示。它以树状的形式表现编程语言的语法结构，树上的每个节点都表示源代码中的一种结构。

听起来还是很绕，没关系，你可以简单理解为 它就是你所写代码的的树状结构化表现形式。

有了这棵树，我们就可以通过操纵这颗树，精准的定位到声明语句、赋值语句、运算语句等等，实现对代码的分析、优化、变更等操作。

AST 在日常业务中也许很难涉及到，有可能你还没有听过，但其实很多时候你已经在使用它了，只是没有太多关注而已，现在流行的 webpack，eslint 等很多插件或者包都有涉及~

抽象语法树在很多领域有广泛的应用，比如浏览器，智能编辑器，编译器。

### 抽象语法树能做什么？

聊到 AST 的用途，其应用非常广泛，下面我简单罗列了一些：

- IDE 的错误提示、代码格式化、代码高亮、代码自动补全等
- JSLint、JSHint 对代码错误或风格的检查等
- webpack、rollup 进行代码打包等
- CoffeeScript、TypeScript、JSX 等转化为原生 Javascript

其实它的用途，还不止这些，如果说你已经不满足于实现枯燥的业务功能，想写出类似 react、vue 这样的牛逼框架，或者想自己搞一套类似 webpack、rollup 这样的前端自动化打包工具，那你就必须弄懂 AST。

先上一些代码；我也在学习中, 有些代码还还在编写中；关于语法树的其他介绍也会慢慢添加；

```javascript
var babylon = require('babylon')

let fileContent = fs.readFileSync(src)
fileContent = fileContent.toString()
let ast = babylon.parse(fileContent, {
  sourceType: 'module',
  plugins: [
    'typescript',
    'classProperties',
    'jsx',
    'trailingFunctionCommas',
    'asyncFunctions',
    'exponentiationOperator',
    'asyncGenerators',
    'objectRestSpread',
    'decorators'
  ]
})
// fix trailingComments issues with hard code
babelTraverse(ast, {
  BlockStatement(path) {
    path.node.body.forEach((item) => {
      if (item.trailingComments && fileContent.charCodeAt([item.end]) === 10) {
        delete item.trailingComments
      }
    })
  }
})
console.log('this is ast', ast)
```

```javascript
// 对 var a = 1 进行解析如下，
{
    "type": "Program",
    "body": [
        {
            "type": "VariableDeclaration",
            "declarations": [
                {
                    "type": "VariableDeclarator",
                    "id": {
                        "type": "Identifier",
                        "name": "a"
                    },
                    "init": {
                        "type": "Literal",
                        "value": 1,
                        "raw": "1"
                    }
                }
            ],
            "kind": "var"
        }
    ],
    "sourceType": "script"
}
// 不过我的就比较多了；我是在处理对react中的页面代码进行解析，简单的结果如下图
```

![image](https://user-images.githubusercontent.com/24740506/92305797-19dd1c80-efbd-11ea-9992-9a4945ed5b93.png)
还在研究中....
