---
title: webpack基础知识 loader && plugin
tags:
  - webpack
author: GleanWu
---

> 在工作中，有个新的需求需要自己写一个 loader 和 plugin；又重新学习相关的知识了；

### loader

`loader就像一个翻译官，将源文件经过转换后生成目标文件并交由下一流程处理`

模块转换器。本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。 因为 Webpack 只认识 JavaScript，所以 Loader 就成了翻译官，对其他类型的资源进行转译的预处理工作。

- 每个 loader 职责都是单一的，就像流水线上的工人
- 顺序很关键（从右往左）

### plugin

`在webpack编译整个生命周期的特定节点执行特定功能`

扩展插件。基于事件流框架 **Tapable** ，插件可以扩展 Webpack 的功能，在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

---

代码补充中....
