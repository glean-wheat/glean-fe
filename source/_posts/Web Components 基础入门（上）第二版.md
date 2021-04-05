---
title: Web Components 基础入门-Web Components概念详解（上）

author: GleanWu

tags:
  - WebComponent
  - 组件化
---

# Web Components 相关概念

`Web Components` 得到越来越多的关注。而且微软的 Edge 开发团队在 2019 年就宣布，在 `Edge` 上开始支持 `Custom Elements` 和 `Shadow Dom`，所有的主流浏览器已经开始支持 `Web Components`。

另外，国外一些公司，像 `Github、 Netflix、 Youtube` 和 `ING` 这样的公司甚至已经在生产环境中使用 `Web Components`。

## 🙋 What are web components?

    Web components 是一套标准，允许开发者创建新的自定义、可重用、封装的 HTML 元素，以便在 Web 页面和 Web
    应用程序中使用。
    自定义组件和小部件建立在 Web components 标准之上，可以跨现代浏览器工作，并且可以与任何 JavaScript 库
    或框架一起使用。

    Web components 基于现有的 Web 标准。支持 Web components 的特性目前正被添加到 HTML 和 DOM 规范中，

    让 web 开发人员可以通过封装样式和自定义行为轻松扩展 HTML。
    来源：https://www.webcomponents.org/introduction

    Web Components 是一套不同的技术组成，允许您创建可重用的定制元素（它们的功能封装在您的代码之外）
    并且在您的web应用中使用它们
    来源： https://developer.mozilla.org/zh-CN/docs/Web/Web_Components

### ✨ 划重点

首先: Web components 是一套标准，有不同的技术组成，允许我们编写模块化、可重用和封装的 HTML 元素。

它最大的**优点**:
由于它们是基于 web 标准的，我们不需要安装任何框架或库就可以开始使用它们。也就是可以通过普通的 javascript 编写 web 组件了！

### 📂 使用

作为开发者，我们都想要尽可能多的重用代码。这对于自定义**UI** 控件来说通常不是那么容易 — 想想复杂的 HTML（以及相关的样式和脚本），有时您不得不写很多代码来呈现自定义 **UI** 控件，在不依赖任何框架的情况下，想要复用自定义的**UI**控件只能不断的粘贴复制相关代码。

Web Components 旨在解决这些问题 — 它由**三项主要技术组成**，它们可以一起使用来创建封装特定功能的定制元素，可以在你喜欢的任何地方重用，不必担心代码或者样式冲突。
基本概念：

### 三项主要技术基本概念：

- **Custom elements（自定义元素）**：一组 JavaScript API，允许您定义 custom elements 及其行为，然后可以在您的用户界面中按照需要使用它们。
- **Shadow DOM（影子 DOM）**：一组 JavaScript API，用于将封装的"影子"DOM 树附加到元素（与主文档 DOM 分开呈现）并控制其关联的功能。通过这种方式，您可以保持元素的功能私有，这样它们就可以被脚本化和样式化，而不用担心与文档的其他部分发生冲突。
- **HTML templates（HTML 模板）**： `<template>` 和 `<slot>` 元素使您可以编写不在呈现页面中显示的标记模板。然后它们可以作为自定义元素结构的基础被多次重用。

## 三项主要技术简单介绍：

### **1.Custom elements（自定义元素）**

一组 `JavaScript API`，允许您定义 `custom elements` 及其行为，然后可以在您的用户界面中按照需要使用它们。简单的解释就是，用户自定义 HTML 元素。它们通过使用`CustomElementRegistry`来注册一个新的元素，通过[window.customElements](https://developer.mozilla.org/zh-cn/docs/web/api/window/customelements)中一个叫 define 的方法来获取注册的实例

```javascript
// 继承 HTMLElement
class MyElement extends HTMLElement {
  constructor() {
    super()
    console.log('this is my-element')
  }
}
window.customElements.define('my-element', MyElement)
```

`window.customElements.define`方法中的第一个参数定义了新创造元素的标签名字，我们可以非常简单的直接使用

```html
<my-element></my-element>
```

为了避免和 native 标签冲突，所以规定自定义标签强制使用中划线来连接。

### **2.Shadow DOM（影子 DOM）**

`Shadow DOM`在`Web Components`开发中是很重要的一组`API`，用于将封装的"影子"`DOM` 树附加到元素中（与主文档 `DOM` 分开呈现）并控制其关联的功能。通过这种方式，您可以保持元素的功能私有化，这样它们就可以被脚本化和样式化，而不用担心与文档的其他部分发生冲突。

`Web components` 的一个重要属性是**封装**——可以将**标记结构**、**样式**和**行为**隐藏起来，并与页面上的其他代码相隔离，保证不同的部分不会混在一起，可使代码更加干净、整洁。其中，`Shadow DOM` 接口是关键所在，它可以将一个隐藏的、独立的 DOM 附加到一个元素上。

使用 `Shadow DOM`，自定义元素的 `HTML` 和 `CSS` 完全封装在组件内。这意味着元素将以单个的 `HTML` 标签出现在文档的 `DOM` 树种。其内部的结构将会放在`#shadow-root`。

下面的图，是截取的个人使用`Web Components`编写的`UI`组件库[wheat-ui](https://github.com/glean-wheat/wheat-ui)，欢迎[start](https://github.com/glean-wheat/wheat-ui)
![image](https://user-images.githubusercontent.com/24740506/99316220-ec56fe00-289e-11eb-9360-d9bd9d50d0b0.png)

`Shadow DOM` 使得`HTML` 和 `CSS` 与**主文档**的`DOM`保持分离。

> 备注： Firefox（从版本 63 开始），Chrome，Opera 和 Safari 默认支持 Shadow DOM。基于 Chromium 的新 Edge 也支持 Shadow DOM；而旧 Edge 未能撑到支持此特性。

#### 为什么要把一些代码和网页上其他的代码分离？

原因之一是，大型站点若 `CSS` 没有良好的组织，导航的样式可能就『泄露』到本不应该去的地方，如主要内容区域，反之亦然。随着站点、应用的拓展，这样的事难以避免。

这里先有一个简单的概念介绍，其中还有它的详细属性以及`css`等相关内容，详情可以查看[个人博客](http://www.wugaoliang.vip/book/webcomponent/shadow-DOM.html)

### **3.HTML templates（HTML 模板）**

`<template>` 和 `<slot>`元素使您可以编写不在呈现页面中显示的标记模板。然后它们可以作为自定义元素结构的基础被多次重用。

```
<template id="userCardTemplate">
  <img src="#" class="image">
  <div class="container">
    <p class="name">User Name</p>
    <p class="email">yourmail@hotmail.com</p>
    <button class="button">confirm</button>
  </div>
</template>

class UserCard extends HTMLElement {
  constructor() {
    super();
    var templateElem = document.getElementById('userCardTemplate');
    var content = templateElem.content.cloneNode(true);
    this.appendChild(content);
  }
}
window.customElements.define('user-card', UserCard);
```

[更多例子](https://github.com/mdn/web-components-examples/。 '更多使用示例')

就像之前的 JQ 一样，当 document.querySelector 开始得到浏览器的广泛支持后，使得 JQ 的使用频率开始变低。这样的事也有可能发生在 Angular 、React、Vue 这些框架上

## 浏览器的支持情况

Edge 在发布的 19 版本中开始提供支持。对于旧的浏览器，可以使用 [polyfill](https://github.com/webcomponents/webcomponentsjs) 来兼容 IE11

![image](https://user-images.githubusercontent.com/24740506/98744397-9856a000-23ec-11eb-9d63-80797ffaa36a.png)
