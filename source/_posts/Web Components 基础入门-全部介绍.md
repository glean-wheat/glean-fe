---
title: Web Components 基础入门-Web Components 概念详解

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

#### 属性

`Shadow DOM` 允许将隐藏的 `DOM` 树附加到常规的 `DOM`树中——它以 `shadow root` 节点为起始根节点，在这个根节点的下方，可以是任意元素，和普通的 `DOM` 元素一样。
![image](https://mdn.mozillademos.org/files/15788/shadow-dom.png)

**这里，有一些 Shadow DOM 特有的术语需要我们了解：**

- **Shadow host**：一个常规 `DOM`节点，`Shadow DOM` 会被附加到这个节点上。
- **Shadow tree**：`Shadow DOM`内部的 DOM 树。
- **Shadow boundary**：`Shadow DOM`结束的地方，也是常规 `DOM`开始的地方。
- **Shadow root**: `Shadow tree`的根节点。

#### 基础用法

可以使用 [Element.attachShadow()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/attachShadow) 方法来将一个 `shadow root` 附加到任何一个元素上。它接受一个配置对象作为参数，该对象有一个 `mode` 属性，值可以是 `open` 或者 `closed`：

```javascript
let shadow = elementRef.attachShadow({ mode: 'open' })
let shadow = elementRef.attachShadow({ mode: 'closed' })
```

`open` 表示可以通过页面内的 `JavaScript` 方法来获取 `Shadow DOM`，例如使用 `Element.shadowRoot` 属性：

```javascript
let myShadowDom = myCustomElem.shadowRoot
```

如果你将一个 `Shadow root` 附加到一个 `Custom element` 上，并且将 `mode` 设置为 `closed`，那么就不可以从外部获取 `Shadow DOM` 了——`myCustomElem.shadowRoot` 将会返回 `null`。浏览器中的某些内置元素就是如此，例如`<video>`，包含了不可访问的 `Shadow DOM`

如果你想将一个 `Shadow DOM` 附加到 `custom element` 上，可以在 `custom element` 的构造函数中添加如下实现（目前，这是 `shadow DOM` 最实用的用法）：

```javascript
let shadow = this.attachShadow({ mode: 'open' })
```

将 `Shadow DOM` 附加到一个元素之后，就可以使用 `DOM APIs` 对它进行操作，就和处理常规 `DOM` 一样。

```javascript
var para = document.createElement('p')
shadow.appendChild(para)
```

#### CSS

`Shadow DOM` 提供了局部作用域的 `CSS`。所有的 `CSS` 都只应用于组件本身。元素将只继承最小数量从组件外部定义的 `CSS`，甚至可以不从外部继承任何 `CSS`。不过你可以暴露这些 `CSS` 属性，以便用户对组件进行样式设置。这可以解决许多 `CSS` 问题，同时仍然允许自定义组件样式。 定义一个 `Shadow root`:

```javascript
const shadowRoot = this.attachShadow({ mode: 'open' })
shadowRoot.innerHTML = `<p>Hello world</p>`
```

这定义了一个带 `mode: open` 的 `Shadow root`，这意味着可以再开发者工具找到它并与之交互，配置暴露出的 `CSS` 属性，监听抛出的事件。同样也可以定义 `mode：closed`，会得到与之相反的表现。

你可以使用使用 `HTML` 字符串添加到 `innerHtml` 的 `property` 属性中，或者使用一个`<template>`去给 `Shadow root` 添加 `HTML`。一个 `HTML` 的 `template` 基本是惰性的 `HTML` 片段，你可以定义了延后使用。在实际插入 `DOM`前，它是不可见也不可解析的。这意味着定义在内部的任何资源都无法获取，任何内部定义的 `CSS` 和 `JavaScript` 只有当它被插入 `DOM` 中时，才会被执行。当组件的 HTML 根据其状态发生更改时，例如你可以定义多个`<template>`元素，然后根据组件的状态去插入这些元素，这样可以轻松的修改组件的 HTML 部分，并不需要修改单个 DOM 节点。

当 Shadow root 被创建之后，你可以使用 `document` 对象的所有 `DOM`方法，例如 `this.shadowRoot.querySelector` 去查找元素。组件的所有样式都被定义在 `style` 标签内，如果你想使用一个常规的`<link rel="stylesheet">`标签，你也可以获取外部样式。除此之外，还可以使用`:host` 选择器对组件本身进行样式设置。例如，自定义元素默认使用 `display: inline`，所以如果你想要将组件展示为款元素，你可以这样做：

```css
:host {
  display: block;
}
```

这还允许你进行上下文的样式化。例如你想要通过 `disabled` 的 `attribute` 来改变组件的背景是否为灰色：

```css
:host([disabled]) {
  opacity: 0.5;
}
```

默认情况下，自定义元素从周围的 `CSS` 中继承一些属性，例如颜色和字体等，如果你想清空组件的初始状态并且将组件内的所有 `CSS`都设置为默认的初始值，你可以使用：

```css
:host {
  all: initial;
}
```

非常重要，需要注意的一点是，从外部定义在组件本身的样式优先于使用:host 在 Shadow DOM 中定义的样式。如果你这样做

```css
my-element {
  display: inline-block;
}
```

它将会被覆盖

```css
:host {
  display: block;
}
```

不应该从外部去改变自定义元素的样式。如果你希望用户可以设置组件的部分样式，你可以暴露 `CSS` 变量去达到这个效果。例如你想让用户可以选择组件的背景颜色，可以暴露一个叫 `--background-color` 的 `CSS` 变量。 假设现在有一个 `Shadow DOM` 的根节点是 `<div id="container">`

```css
#container {
  background-color: var(--background-color);
}
```

现在用户可以在组件的外部设置它的背景颜色

```css
my-element {
  --background-color: #ff0000;
}
```

你还可以在组件内设置一个默认值，以防用户没有设置

```css
:host {
  --background-color: #ffffff;
}

#container {
  background-color: var(--background-color);
}
```

当然你还可以让用户设置任何的 `CSS` 变量，前提是这些变量的命名要以 **--开头**。

通过提供局部的 `CSS、HTML，Shadow DOM` 解决了全部 `CSS` 可能带来的一些问题，这样问题通常导致不断地添加样式表，其中包含了越来越多的选择器和覆盖。`Shadow DOM` 似的标记和样式捆绑到自己的组件内，而不需要任何工具和命名约定。你再也不用担心新的 `class` 或 `id` 会与现有的任何一个冲突。

这里先有一个简单的概念介绍，[更多>>](http://www.wugaoliang.vip/book/webcomponent/shadow-DOM.html)

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

另外在 `Shadow DOM`中提到使用`this.shadowRoot.innerHTML` 来向一个元素的 `shadow root` 添加 `HTML`，有了`<template>`，你也可以使用 `<template>`来做。`template` 保存 `HTML` 供以后使用。它不会被渲染，并只有确保内容是有效的才会进行解析。模板中的 `JavaScript` 不会被执行，也会获取任何外部资源，默认情况下它是隐藏的。

当一个 `web component` 需要根据不同的情况来渲染不同的标记时，可以用不同的模板来完成。

为了方便大家理解，继续拿个人使用`Web Components`编写的`UI`组件库[wheat-ui](https://github.com/glean-wheat/wheat-ui)举例；
在[wheat-ui](https://github.com/glean-wheat/wheat-ui) 的 [button](https://github.com/glean-wheat/wheat-ui/blob/master/src/button/button.js#L372)组件 中 因为`button`组件会根据`href`属性渲染不同的标签，如果存在`href`属性就渲染为`a`标签

```javascript
// 同时创建了两个标签
const template = document.createElement('template')
const templateTagA = document.createElement('template')
template.innerHTML = `
  <div class="wheat-button-container">
    <button class='wheat-button'>
      <slot name='icon'/>
      Label
    </button>
  </div>
`
templateTagA.innerHTML = `
  <div class="wheat-button-container">
    <a class='wheat-button'><slot name='icon'/>链接</a>
  </div>
`
// 在渲染的时候，会根据href熟悉去渲染不同的template

this._shadowRoot = this.attachShadow({ mode: 'open' })
this._shadowRoot.appendChild(this.href ? templateTagA.content.cloneNode(true) : template.content.cloneNode(true))
```

#### template 中关键 API 介绍

##### cloneNode

> Node.cloneNode() 方法返回调用该方法的节点的一个副本.

    语法：var dupNode = node.cloneNode(deep);

- node
  将要被克隆的节点

- dupNode
  克隆生成的副本节点

- deep 可选
  是否采用深度克隆,如果为 `true`,则该节点的所有后代节点也都会被克隆,如果为 `false`,则只克隆该节点本身.

克隆一个元素节点会拷贝它所有的属性以及属性值,当然也就包括了属性上绑定的事件(比如 `onclick="alert(1)"`),但不会拷贝那些使用 `addEventListener()`方法或者 `node.onclick = fn` 这种用 `JavaScript` 动态绑定的事件.

在使用 `Node.appendChild()`或其他类似的方法将拷贝的节点添加到文档中之前,那个拷贝节点并不属于当前文档树的一部分,也就是说,它没有父节点.

如果 `deep` 参数设为 `false`,则不克隆它的任何子节点.该节点所包含的所有文本也不会被克隆,因为文本本身也是一个或多个的 `Text` 节点.

如果 `deep` 参数设为 `true`,则会复制整棵 `DOM` 子树(包括那些可能存在的 `Text` 子节点).对于空结点(例如`<img>`和`<input>`元素),则 `deep` 参数无论设为 true 还是设为 false,都没有关系,但是仍然需要为它指定一个值.

关于使用`cloneNode`的目的是；获取`<template>`节点以后，克隆了它的所有子元素，这是因为可能有多个自定义元素的实例，这个模板还要留给其他实例使用，所以不能直接移动它的子元素。

#### 添加样式

自定义元素还没有样式，可以给它指定全局样式，比如下面这样。

```css
wheat-button {
  /* ... */
}
```

但是，组件的样式应该与代码封装在一起，只对自定义元素生效，不影响外部的全局样式。所以，可以把样式写在`<template>`里面。在[wheat-ui](https://github.com/glean-wheat/wheat-ui) 的 [button](https://github.com/glean-wheat/wheat-ui/blob/master/src/button/button.js#L372)组件;

```javascript
// 设置样式
const style = `
.wheat-button {
  /* ... */
}
`
// 在template中使用
const template = document.createElement('template')
const templateTagA = document.createElement('template')
template.innerHTML = `
    ${style} // 重点是这一行
  <div class="wheat-button-container">
    <button class='wheat-button'>
      <slot name='icon'/>
      Label
    </button>
  </div>
`
templateTagA.innerHTML = `
  <div class="wheat-button-container">
    <a class='wheat-button'><slot name='icon'/>链接</a>
  </div>
`
```

以上简单的介绍了
[更多例子](https://github.com/mdn/web-components-examples/。 '更多使用示例')

## 浏览器的支持情况

Edge 在发布的 19 版本中开始提供支持。对于旧的浏览器，可以使用 [polyfill](https://github.com/webcomponents/webcomponentsjs) 来兼容 IE11

![image](https://user-images.githubusercontent.com/24740506/98744397-9856a000-23ec-11eb-9d63-80797ffaa36a.png)

---

title: Web Components 基础入门-生命周期以及事件触发（中）

author: GleanWu

tags:

- WebComponent
- 组件化

---

中篇

# Web Components 生命周期

上一篇中简单的介绍了一下一些关键名词概念：`Custom Elements`、`Shadow DOM`、`HTML templates`接下来如果想要开始着手开发的话，有一个比较重要的知识点。关于自定义元素的 **生命周期**以及**自定义标签中的事件触发机制**

## ♻️ 组件的生命周期

让我们看一下自定义元素的生命周期，下面是一份代码备注，方便开始快速开发。

```javascript
class MyElementLifecycle extends HTMLElement {
  // 元素初始化的时候执行
  constructor() {
    // HTMLElement.prototype.constructor.call(this)
    super()
    console.log('constructed!')
  }
  /**
   * connectedCallback
   * 当元素插入到 DOM 中时，将调用 connectedCallback。
   * 这是运行安装代码的好地方，比如获取数据或设置默认属性。
   * 可以将其与React的componentDidMount方法进行比较
   * vue的mount方法作比较
   */
  connectedCallback() {
    console.log('connected!')
  }
  /**
   * disconnectedCallback
   * 只要从 DOM 中移除元素，就会调用 disconnectedCallback。清理时间到了！
   * 我们可以使用 disconnectedCallback 删除事件监听，或取消记时。
   * 但是请记住，当用户直接关闭浏览器或浏览器标签时，这个方法将不会被调用。
   *
   * 可以用window.unload beforeunload或者widow.close 去触发在浏览器关闭是的回调
   *
   * 可以与 react 中的 componentWillUnmount 的方法进行比较
   * vue 中的 destory中是生命周期函数进行对比
   */
  disconnectedCallback() {
    console.log('disconnected!')
  }

  static get observedAttributes() {
    return ['my-attr']
  }
  /**
   *
   * @param {*} name
   * @param {*} oldVal
   * @param {*} newVal
   *
   * 每当添加到observedAttributes数组的属性发生变化时，就会调用这个函数。使用属性的名称、旧值和新值调用该方法
   * react 中的 static getDerivedStateFromProps(props, state) 有些类似
   * 基本上和vue中的watch使用和observedAttributes + attributeChangedCallback使用雷同；
   */

  attributeChangedCallback(name, oldVal, newVal) {
    console.log(`Attribute: ${name} changed!`)
  }
  /**
   * 每次将自定义元素移动到新文档时，都会调用 adoptedCallback。只有当您的页面中有 < iframe > 元素时，您才会遇到这个用例。
   * 通过调用document.adoptnode (element)调用它，基本上用不上
   */
  adoptedCallback() {
    console.log('adopted!')
  }
  /**
   * 生命周期的执行顺序  挂载的时候 按照react 或者vue中的执行顺序是相同的
   * constructor -> attributeChangedCallback -> connectedCallback
   */
}
// 不是生命周期的API 但是非常重要 注册
window.customElements.define('my-element-lifecycle', MyElementLifecycle)
```

### 下面是详细介绍：

#### constructor() 初始化

`constructor()`方法是元素挂载之前运行的，一般会在这个函数中设置一些元素的**初始状态**，还有**事件的监听** 以及创建 `Shadow Dom`

> 类比 `Vue` 中的 `create()`，`React`中 `class` 组件中的 `constructor()`它们两个是一样的

#### connectedCallback() 挂载完成

当元素插入到 DOM 中时，将调用 `connectedCallback`， 通常，组件的一些`样式`或者其他需要`获取元素的状态` 应该在 `connectedCallback` 进行设置，因为在这里才能确定元素的所有属性和子元素都是可用的。`constructor` 一般应该只用于初始化状态和设置`shadow DOM`。

##### `constructor` 和 `connectedCallback` 之间的区别

创建元素时调用`constructor`，
已经插入到 `DOM` 元素后调用 `connectedCallback`

> 可以将其与`vue`的`mount`方法、`React`的`componentDidMount`方法进行比较

#### disconnectedCallback 卸载元素

当`自定义元素`从`DOM`中移除的时候，就会触发该方法；同时也会在这个事件中删除调改元素添加的事件，比如全局的`键盘函数`以及`延时函数`，
disconnectedCallback 的对应函数是 connectedCallback，
另外有个注意点，当用户关闭浏览器或关闭该标签页时，将不会调用此方法。不过可以用 `window.unload beforeunload` 或者 `widow.close` 去触发在浏览器关闭是的回调

> `react` 中的 `componentWillUnmount` 的方法进行比较, `vue` 中的 `destory` 中是生命周期函数进行对比

#### attributeChangedCallback 属性监听

使用这个函数需要和 `observedAttributes` 该方法配合使用，需要将你要监听的属性在该方法中注册；

```javascript
static get observedAttributes() {
    return ['my-attr']
}

```

上面的函数中，就是对`my-attr`属性进行了监听；当该属性的值发生改变的时候，就会触发`attributeChangedCallback`

```javascript
/**
   *
   * @param {*} name 属性名称
   * @param {*} oldVal 改变前的值
   * @param {*} newVal 改变后的值
   *
   */
attributeChangedCallback(name, oldVal, newVal) {
    console.log(`Attribute: ${name} changed!`)
}
```

**注意：**只有在 `observedAttributes` 监听的值才会触发 `attributeChangedCallback`

> `react` 中的 `static getDerivedStateFromProps(props, state)` 与这个有些类似，以及 `vue` 中的 `watch` 函数

#### 执行顺序

`constructor -> attributeChangedCallback -> connectedCallback`；vue 以及 react 中的生命周期也是这样的一个顺序；

这个时候，可能会有些困惑了。

###### 为什么 `attributeChangedCallback` 要在 `connectedCallback` 之前执行 ?

回想一下，web 组件上的属性主要用来初始化配置。这意味着当组件被插入`DOM`时，这些配置需要可以被访问了。因此`attributeChangedCallback`要在`connectedCallback`之前执行。 这意味着你需要根据某些属性的值，在`Shadow DOM`中配置任何节点，那么你需要在构造函数中引用这些节点，而不是在`connectedCallback`中引用它们。

例如，如果你有一个`ID`为`container`的组件，并且你需要在根据属性的改变来决定是否给这个元素添加一个灰色的背景，那么你可以在构造函数中引用这个元素，以便它可以在`attributeChangedCallback`中使用：

```javascript
constructor() {
  this.container = this.shadowRoot.querySelector('#container');
}
attributeChangedCallback(attr, oldVal, newVal) {
  if(attr === 'disabled') {
    if(this.hasAttribute('disabled') {
      this.container.style.background = '#808080';
    }
    else {
      this.container.style.background = '#ffffff';
    }
  }
}


```

如果你一直等到`connectedCallback`再去创建`this.container`。然后在第一时间调用`attributeChangedCallback`，它还是不可用的。因此尽管你应该尽可能的延后你组件的`connectedCallback`，但在这种情况下是不可能的。

##### customels.define

另外，在自定义元素未注册时候使用它，它会仅仅作为 `HTMLUnknownElement`，一个实例；代表着一个无效的 HTML 元素，派生自 HTMLElement 接口,，但它没有任何可用的附加属性或方法

然后通过 `customements.define()` 注册时，它会继承`HTMLElement`这个类。这个过程称为升级。
通过 `customElements.whenDefined`方法，当自定义元素被定义时一个 `Promise` 返回`{jsxref("undefined")}}`. 如果自定义元素已经被定义，则 `resolve` 立即执行

```javascript

Promise<> customElements.whenDefined(name);

```

```javascript
customElements.whenDefined('my-element').then(() => {
  // my-element is now defined
})
```

## 常规事件监听

以`<wheate-button></wheate-button>`为例，[源码地址](https://github.com/glean-wheat/wheat-ui/tree/master/src/button)

我们需要使我们的自定义元素工作时，单击它。首先，自定义元素可以注册一个事件侦听器来对用户的交互作出反应。例如，我们可以在按钮上添加一个事件监听器:

```javascript
class WheatButton extends HTMLElement {
  constructor() {
    super()

    this._shadowRoot = this.attachShadow({ mode: 'open' })
    this._shadowRoot.appendChild(template.content.cloneNode(true))
    this.$button = this._shadowRoot.querySelector('button')
    this.$button.addEventListener('click', () => {
      // do something
    })
  }
}
```

从元素外部添加这个监听器也是可以

```html
<wheat-button onclick="showModal()">打开弹框</wheat-button>
```

这样也是可以触发正常触发的，毕竟是从`HTMLElement`这个类上继承的，所以关于标签一些基础的属性和方法都是同时具备的；
而且不需要在自定元素中做任何的处理；但是，在定制元素内部定义它，可以更好地控制应该传递给外部注册的函数内容。

将外部用户自定的函数传递给自定义元素，有很多方法。React、Vue 中都对此进行了处理，按照框架定义标准即可，整个处理起来还是比较麻烦的。
不过我的想法是没有必要处理，而且我希望能尽可能的使用原生方法；

我们刚刚定义了一个 onClick 处理程序作为元素的函数。接下来，我们可以在自定义元素中的监听调用这个函数属性:

```javascript
class WheatButton extends HTMLElement {
  constructor() {
    super()

    this._shadowRoot = this.attachShadow({ mode: 'open' })
    this._shadowRoot.appendChild(template.content.cloneNode(true))
    this.$button = this._shadowRoot.querySelector('button')
    this.$button.addEventListener('click', () => {
      // do something
    })
    this.$button.addEventListener('click', () => {
      this.onClick('Hello from within the Custom Element')
    })
  }
}
```

在使用的时候

```javascript
<whate-button label="Click Me"></whate-button>

<script>
// 方式1
  document.querySelector('wheat-button').onClick = value => {
      console.log(value)
    }

// 方式2
document
    .querySelector('my-button')
    .addEventListener('click', value => console.log(value));
</script>
```

# 自定义标签中的事件

## CustomEvent

在组件中自定义事件作为组件的 API 是必不可少的；
可以使用 [CustomEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/CustomEvent) 从自定义元素抛出任何事件

通过 `CustomEvent` 事件是由程序创建的，可以有任意自定义功能的事件。

```javascript
class MyElement extends HTMLElement {
  ...  connectedCallback() {
    this.dispatchEvent(new CustomEvent('custom', {
      detail: {message: 'a custom event'}
    }));
  }
}

// 在使用标签的地方

document.querySelector('my-element').addEventListener('custom', e =>
console.log('message from event:', e.detail.message)
);
```

## 整个内容比较简单，这样可以自定方法为组件自定义标签服务。同时也能方便传递参数。

title: Web Components 基础入门 - 编写自定义标签 wheat-modal（下）

author: GleanWu

tags:

- WebComponent
- 组件化

---

经过基础的`api`介绍，演示如何把这个弹层，写成 `Web Components` 组件，这里是最后的[完整代码](https://github.com/glean-wheat/wheat-ui/tree/master/modal)。

![image](https://user-images.githubusercontent.com/24740506/99131610-9be96180-264e-11eb-90ed-a5c8065209dc.png)

网页只要插入下面的代码，就可以正常使用弹层

```html
<wheat-modal></wheat-modal>
```

这种自定义的 HTML 标签，称为自定义元素（`custom element`）。根据规范，自定义元素的名称必须包含连词线，`<wheat-modal>`不能写成`<wheatmodal>`，这样做是为了避免元素名称冲突，用与区别原生的 `HTML` 元素。自定义元素也不能自动关闭，`HTML` 只允许少数元素自动关闭。 因为 `HTML` 解析对对安全方面有很高的要求。

## 二、customElements.define()

自定义元素需要使用 `JavaScript` 定义一个类，所有 `<wheate-modal>` 都会是这个类的实例。

```javascript
class WheatModal extends HTMLElement {
  constructor() {
    super()
  }
}
```

上面代码中，`WheatModal` 就是自定义元素的类。注意，这个类的父类是 `HTMLElement`，因此继承了 HTML 元素的特性。

接着，使用浏览器原生的 `customElements.define()`方法，告诉浏览器`<wheat-modal>`元素与这个类关联。

```javascript
window.customElements.define('wheat-modal', WheatModal)
```

## 三、自定义元素的内容

自定义元素<wheate-modal>目前还是空的，下面在类里面给出这个元素的内容。

```javascript
class WheatModal extends HTMLElement {
  constructor() {
    super()
    this.render()
  }

  render() {
    this.template = document.createElement('div')
    this.template.innerHTML = `
          <div class="wheat-modal">
          <div class="wheat-modal-mask"></div>
          <div class="wheat-modal-container">
            <div class="wheat-modal-wrapper">
              <div class="wheat-modal-header">
                <span class="wheat-modal-header-text"></span>
                <span class="wheat-modal-header-close">X</span>
              </div>
              <div class="wheat-modal-content">
                <slot name="content">
                  content
                </slot>
              </div>
              <div class="wheat-modal-footer">
                <slot name='wheat-modal-footer'>
                  <wheat-button class="wheat-modal-footer-cancel" type='cancel'>取消</wheat-button>
                  <wheat-button class="wheat-modal-footer-confirm" type='confirm'>确定</wheat-button>
                </slot>
              </div>
            </div>
          </div>
        </div>`
    this.append(this.template)
  }
}
```

上面代码`render()`函数的最后一行，`this.append()`的 `this` 表示自定义元素实例。

完成这一步以后，自定义元素内部的 `DOM` 结构就**已经生成**了。

## 四、`<template>标签`

使用 `JavaScript` 写上一节的 DOM 结构很麻烦，`Web Components API` 提供了`<template>`标签，可以在它里面使用 `HTML` 定义 `DOM` 以及 `css`

我们一步一步来。我们首先通过调用

```javascript
const template = document.createElement('template')
```

创建一个 `<template >` ，然后在其中设置一些 `HTML`。

```javascript
const WheatModaltemplate = document.createElement('template')
WheatModaltemplate.innerHTML = `
<div class="wheat-modal">
  <div class="wheat-modal-mask"></div>
  <div class="wheat-modal-container">
    <div class="wheat-modal-wrapper">
      <div class="wheat-modal-header">
        <span class="wheat-modal-header-text"></span>
        <span class="wheat-modal-header-close">X</span>
      </div>
      <div class="wheat-modal-content">
        <slot name="content">
          content
        </slot>
      </div>
      <div class="wheat-modal-footer">
        <slot name='wheat-modal-footer'>
          <wheat-button class="wheat-modal-footer-cancel" type='cancel'>取消</wheat-button>
          <wheat-button class="wheat-modal-footer-confirm" type='confirm'>确定</wheat-button>
        </slot>
      </div>
    </div>
  </div>
</div>
`
```

然后，改写一下自定义元素的类，为自定义元素加载`<template>`

```javascript
class WheatModal extends HTMLElement {
  constructor() {
    super()
    this.render()
  }

  render() {
    this._shadowRoot = this.attachShadow({ mode: 'open' })
    this._shadowRoot.appendChild(WheatModaltemplate.content.cloneNode(true))
  }
}
```

这里可以看到，`render` 方法发生了改变
上面代码中，获取`<template>`节点以后，`克隆`了它的所有子元素，这是因为可能有多个自定义元素的实例，这个模板还要留给其他实例使用，所以不能直接移动它的子元素。另外，我们只在模板上设置 `innerHTML` 一次。我们使用模板的原因是克隆模板比调用要容易，消耗性能较少

[完整代码](https://github.com/glean-wheat/wheat-ui/)

## 五、添加样式

自定义元素还没有样式，可以给它指定全局样式，比如下面这样。

```css
wheat-modal {
  /*....*/
}
```

但是，组件的样式应该与`代码封装`在一起，只对自定义元素生效，不影响外部的全局样式。所以，可以把样式写在`<template>`里面。

```javascript
const WheatModaltemplate = document.createElement('template')
WheatModaltemplate.innerHTML = `
<style>
:host{
  display: block;
}
.wheat-modal-mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  height: 0;
  z-index: 1000;
  background: rgba(0, 0, 0, 0.45);
  transition: opacity 0.3s, height 0s 0.3s;
}
.wheat-modal-mask-show {
  opacity: 1;
  height: 100%;
  transition: opacity 0.3s;
}
.wheat-modal-container {
  z-index: 1000;
  top: 100px;
  left: 50%;
  transform: translateX(-50%);
  position: fixed;
}

.wheat-modal-header {
  border-bottom: 1px solid #e6e6e6;
  font-size: 16px;
  color: #333;
  font-weight: 600;
  height: 54px;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  padding: 0 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-shrink: 0;
}
.wheat-modal-wrapper {
  width: 600px;
  max-height: 600px;
  min-height: 240px;
  display: flex;
  flex-direction: column;
  background: #fff;
  border-radius: 2px;
  box-shadow: 0 5px 15px 0 rgba(0, 0, 0, 0.1);
}
.wheat-modal-wrapper-show {
  animation:scale 0.3s 1;
}
@keyframes scale
{
  from {transform: scale(0.4);opacity: 0;}
  to {opacity: 1;transform: scale(1);}
}
.wheat-modal-header-close {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em !important;
  overflow: hidden;
  cursor: pointer;
}
.wheat-modal-footer {
  border-top: 1px solid #e6e6e6;
  height: 54px;
  padding: 0 24px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  flex-shrink: 0;
}
.wheat-modal-content {
  box-sizing: border-box;
  flex: 1;
  overflow: auto;
  padding: 24px;
}

</style>
<div class="wheat-modal">
  <div class="wheat-modal-mask"></div>
  <div class="wheat-modal-container">
    <div class="wheat-modal-wrapper">
      <div class="wheat-modal-header">
        <span class="wheat-modal-header-text"></span>
        <span class="wheat-modal-header-close">X</span>
      </div>
      <div class="wheat-modal-content">
        <slot name="content">
          content
        </slot>
      </div>
      <div class="wheat-modal-footer">
        <slot name='wheat-modal-footer'>
          <wheat-button class="wheat-modal-footer-cancel" type='cancel'>取消</wheat-button>
          <wheat-button class="wheat-modal-footer-confirm" type='confirm'>确定</wheat-button>
        </slot>
      </div>
    </div>
  </div>
</div>
`
```

上面代码中，`<template>`样式里面的`:host` 伪类，指代自定义元素**本身**。

## 六、自定义元素的参数

`<wheat-modal>`内容现在是在`<template>`里面设定的，为了方便使用，把它改成参数。

```html
<wheat-modal title="弹窗" visiable="false" maskCloseable="true">
  <div slot="content">弹框内容</div>
  <!-- <div slot="modal-footer">
        自定义footer
      </div> -->
</wheat-modal>
```

`wheat-modal`也要进行相应的改造

```javascript
class WheatModal extends HTMLElement {
  constructor() {
    super()
    this.data = {
      title: this.getAttribute('title') || '弹层组件',
      visiable: this.getAttribute('visiable'),
      closeable: this.getAttribute('closeable') || 'true',
      maskCloseable: this.getAttribute('maskCloseable') || 'true'
    }
    this.renderShadowDom()
    this.$modalRoot = this._shadowRoot.querySelector('.wheat-modal')
    this.$closeBtn = this._shadowRoot.querySelector('.wheat-modal-header-close')
    this.$wrapper = this._shadowRoot.querySelector('.wheat-modal-wrapper')
    console.log('this.data.visiable', this.data.visiable)
    this.$mask = this._shadowRoot.querySelector('.wheat-modal-mask')

    this.bindEvents()
  }
  static get observedAttributes() {
    return ['visiable', 'title']
  }
  attributeChangedCallback(name, oldVal, newVal) {
    this.$modalRoot.style.display = name === 'visiable' && newVal !== 'false' ? 'block' : 'none'
    if (name === 'visiable' && newVal !== 'false') {
      this.$mask.classList.add('wheat-modal-mask-show')
      this.$wrapper.classList.add('wheat-modal-wrapper-show')
    }
  }

  connectedCallback() {
    this._shadowRoot.querySelector('.wheat-modal-header-text').innerHTML = this.data.title
    this._shadowRoot.querySelector('.wheat-modal-content')
    console.log(this.data)
    this.$closeBtn.style.display = this.data.closeable ? 'display' : 'none'
  }
  bindEvents() {
    this.hide()
    this.show()
  }
  disconnectedCallback() {
    this.removeEventListener('keydown', this._onKeyDown)
    this.removeEventListener('click', this._onClick)
  }
  hide() {
    this.$cancelBtn = this._shadowRoot.querySelector('.wheat-modal-footer-cancel')

    // 添加自定义事件
    this.$cancelBtn.addEventListener('click', () => {
      this.dispatchEvent(
        // 自定义事件
        new CustomEvent('onCancel', {
          detail: { visiable: false }
        })
      )
    })

    // 添加自定义事件
    this.$closeBtn.addEventListener('click', () => {
      this.dispatchEvent(
        // 自定义事件
        new CustomEvent('onCancel', {
          detail: { visiable: false }
        })
      )
    })
    this.data.maskCloseable === 'true' &&
      this.$mask.addEventListener('click', () => {
        this.$modalRoot.style.display = 'none'
      })
  }
  show() {
    this.$confirmBtn = this._shadowRoot.querySelector('.wheat-modal-footer-confirm')
    // 添加自定义事件
    this.$confirmBtn.addEventListener('click', () => {
      this.dispatchEvent(
        // 自定义事件
        new CustomEvent('onConfirm', {
          detail: { isConfirmed: true }
        })
      )
    })
  }
  renderShadowDom() {
    this._shadowRoot = this.attachShadow({ mode: 'open' })
    this._shadowRoot.appendChild(WheatModaltemplate.content.cloneNode(true))
  }
}
// this需要讲解
window.customElements.define('wheat-modal', WheatModal)
```

## 在 html 中使用

### 将 刚才写的 wheat-modal.js 通过 script 引入即可

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Modal</title>
  </head>
  <style></style>
  <body>
    <button onclick="showModal()">显示弹框</button>
    <wheat-modal title="弹窗" visiable="false" maskCloseable="true">
      <div slot="content">
        弹框内容
      </div>
    </wheat-modal>
  </body>
  <script src="wheat-modal.js"></script>
  <script>
    const MyModalDom = document.querySelector('wheat-modal')

    MyModalDom.addEventListener('onCancel', (value) => {
      const {
        detail: { visiable }
      } = value
      console.log('触发取消方法')
      MyModalDom.setAttribute('visiable', visiable)
    })

    MyModalDom.addEventListener('onConfirm', (value) => {
      console.log('触发确定方法')
      MyModalDom.setAttribute('visiable', false)
    })
    const showModal = () => {
      MyModalDom.setAttribute('visiable', true)
    }
  </script>
</html>
```

## 在 angular、vue、react 项目中使用

## 在 React 中使用

```javascriptx
import React, { useState, useEffect } from 'react'
import 'wheat-modal.js'
const App = () => {
  const [visiable, setVisiable] = useState(false)
  useEffect(() => {
    const MyModalDom = document.querySelector('wheat-modal')
    MyModalDom.addEventListener('onCancel', (value) => {
      const {
        detail: { visiable }
      } = value
      console.log('触发取消方法')
      setVisiable(visiable)
    })

    MyModalDom.addEventListener('onConfirm', (value) => {
      console.log('触发确定方法')
      setVisiable(false)
    })
  }, [])
  return (
    <div className="App">
      <button
        onClick={() => {
          setVisiable(true)
        }}
      >
        显示弹框
      </button>
      <wheat-modal title="title" visiable={visiable}>
        <div slot="content">弹框内容</div>
      </wheat-modal>
    </div>
  )
}

export default App
```

## 在 Vue 中使用

```vue
<template>
  <div>
    <button @click="showModal">
      显示弹框
    </button>
    <wheat-modal title="title" :visiable="visiable.toString()">
      <div slot="content">弹框内容</div>
    </wheat-modal>
    <div>{{ visiable }}</div>
  </div>
</template>

<script>
import 'wheat-modal.js'

export default {
  data() {
    return {
      visiable: false
    }
  },
  mounted() {
    const MyModalDom = document.querySelector('wheat-modal')
    MyModalDom.addEventListener('onCancel', (value) => {
      const {
        detail: { visiable }
      } = value
      console.log('触发取消方法', value)
      this.visiable = visiable
    })

    MyModalDom.addEventListener('onConfirm', (value) => {
      console.log('触发确定方法', value)
      this.visiable = false
      this.hidden()
    })
  },
  methods: {
    showModal() {
      this.visiable = true
    },
    hidden() {
      this.visiable = false
    }
  }
}
</script>
```

[更多>>](https://github.com/glean-wheat/wheat-ui/tree/master/src)

如果感兴趣的小伙伴欢迎点赞，后续还有更多相关内容持续更新，希望得到大家的支持
