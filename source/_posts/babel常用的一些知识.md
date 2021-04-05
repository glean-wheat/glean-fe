---
title: babel常用的一些知识
author: GleanWu
tags:
  - webpack
  - babel
---

## Babel 的包构成

### 核心包

- babel-core：babel 转译器本身，提供了 babel 的转译 API，如 babel.transform 等，用于对代码进行转译。像 webpack 的 babel-loader 就是调用这些 API 来完成转译过程的。
- babylon：js 的词法解析器
- babel-traverse：用于对 AST（抽象语法树，想了解的请自行查询编译原理）的遍历，主要给 plugin 用
- babel-generator：根据 AST 生成代码 ###功能包
- babel-types：用于检验、构建和改变 AST 树的节点
- babel-template：辅助函数，用于从字符串形式的代码来构建 AST 树节点
- babel-helpers：一系列预制的 babel-template 函数，用于提供给一些 plugins 使用
- babel-code-frames：用于生成错误信息，打印出错误点源代码帧以及指出出错位置
- babel-plugin-xxx：babel 转译过程中使用到的插件，其中 babel-plugin-transform-xxx 是 transform 步骤使用的
- babel-preset-xxx：transform 阶段使用到的一系列的 plugin
- babel-polyfill：JS 标准新增的原生对象和 API 的 shim，实现上仅仅是 core-js 和 regenerator-runtime 两个包的封装
- babel-runtime：功能类似 babel-polyfill，一般用于 library 或 plugin 中，因为它不会污染全局作用域
  工具包
- babel-cli：babel 的命令行工具，通过命令行对 js 代码进行转译
- babel-register：通过绑定 node.js 的 require 来自动转译 require 引用的 js 代码文件

### babel 的工作原理

babel 是一个转译器，感觉相对于编译器 compiler，叫转译器 transpiler 更准确，因为它只是把同种语言的高版本规则翻译成低版本规则，而不像编译器那样，输出的是另一种更低级的语言代码。

但是和编译器类似，babel 的转译过程也分为三个阶段：`parsing`、`transforming`、`generating`，以 ES6 代码转译为 ES5 代码为例，babel 转译的具体过程如下：

```
ES6代码输入 ==》 babylon进行解析 ==》 得到AST
==》 plugin用babel-traverse对AST树进行遍历转译 ==》 得到新的AST树
==》 用babel-generator通过AST树生成ES5代码
```

此外，还要注意很重要的一点就是，babel 只是转译新标准引入的语法，比如 ES6 的箭头函数转译成 ES5 的函数；而新标准引入的新的原生对象，部分原生对象新增的原型方法，新增的 API 等（如 Proxy、Set 等），这些 babel 是不会转译的。需要用户自行引入 polyfill 来解决

### plugins

插件应用于 babel 的转译过程，尤其是第二个阶段 transforming，如果这个阶段不使用任何插件，那么 babel 会原样输出代码。
我们主要关注 transforming 阶段使用的插件，因为 transform 插件会自动使用对应的词法插件，所以 parsing 阶段的插件不需要配置。

### presets

如果要自行配置转译过程中使用的各类插件，那太痛苦了，所以 babel 官方帮我们做了一些预设的插件集，称之为 preset，这样我们只需要使用对应的 preset 就可以了。以 JS 标准为例，babel 提供了如下的一些 preset：

es2015
es2016
es2017
env
es20xx 的 preset 只转译该年份批准的标准，而 env 则代指最新的标准，包括了 latest 和 es20xx 各年份
另外，还有 stage-0 到 stage-4 的标准成形之前的各个阶段，这些都是实验版的 preset，建议不要使用。

### polyfill

polyfill 是一个针对 ES2015+环境的 shim，实现上来说 babel-polyfill 包只是简单的把 core-js 和 regenerator runtime 包装了下，这两个包才是真正的实现代码所在（后文会详细介绍 core-js）。
使用 babel-polyfill 会把 ES2015+环境整体引入到你的代码环境中，让你的代码可以直接使用新标准所引入的新原生对象，新 API 等，一般来说单独的应用和页面都可以这样使用。
