---
title: WebComponent 个人理解
tags:
  - WebComponent
  - 组件化
author: GleanWu
---

### 接下来自己开始依照个人计划开始 WebComponent 相关技术的深入研究

Web Components 是一套不同的技术，允许您创建可重用的定制元素（它们的功能封装在您的代码之外）并且在您的 web 应用中使用它们。

---

Web Component 的诞生本身是为了更加方便的去定义组件，它有着天然的样式隔离和作用域空间。这也符合了组件的最基本的特点：** 高内聚、低耦合 **。

一个好的组件库，本身肯定有着自己的样式管理、工具类管理等规范。根本目的就是要做的相互隔离，不受其他的样式和功能影响。但是实际的开发中，有些情况是避免不了的；复杂度、专属语法、性能消耗的代价。

一般在开发中大家都会对组件库进行定制化的模改；样式、字体大小等方面。这些都涉及到了样式方面的改动

---

现代浏览器的 API 已经更新到你不需要使用一个框架就可以去创建一个可服用的组件。Custom Element 和 Shadow DOM 都可以让你去创造可复用的组件。

最早在 2011 年，Web Components 就已经是一个只需要使用 HTML、CSS、JavaScript 就可以创建可复用的组件被介绍给大家。这也意味着你可以不使用类似 React 和 Angular 的框架就可以创造组件。甚至，这些组件可以无缝的接入到这些框架中。

这么久以来第一次，我们可以只使用 HTML、CSS、JavaScript 来创建可以在任何现代浏览器运行的可复用组件。Web Components 现在已经被主要的浏览器的较新版本所支持。

Edge 将会在接下来的 19 版本提供支持。而对于那些旧的版本可以使用 polyfill 兼容至 IE11. 这意味着你可以在当下基本上任何浏览器甚至移动端使用 Web Components。

创造一个你定制的 HTML 标签，它将会继承 HTM 元素的所有属性，并且你可在任何支持的浏览器中通过简单的引入一个 script。所有的 HTML、CSS、JavaScript 将会在组件内部局部定义。 这个组件在你的浏览器开发工具中显示为一个单独个 HTML 标签，并且它的样式和行为都是完全在组件内进行，不需要工作区，框架和一些前置的转换。

是不是有些心动了；

Web Components 的一些主要功能：
未完。。。
