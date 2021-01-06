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

    Web components 是一套标准，允许开发者创建新的自定义、可重用、封装的 HTML 元素，以便在 Web 页面和 Web 应用程序中使用。
    自定义组件和小部件建立在 Web components 标准之上，可以跨现代浏览器工作，并且可以与**任何 JavaScript 库或框架**一起使用。
    Web components 基于现有的 Web 标准。支持 Web components 的特性目前正被添加到 HTML 和 DOM 规范中，
    让 web 开发人员可以通过封装样式和自定义行为轻松扩展 HTML。
    来源：https://www.webcomponents.org/introduction

    Web Components 是一套不同的技术组成，允许您创建可重用的定制元素（它们的功能封装在您的代码之外）并且在您的web应用中使用它们
    来源： https://developer.mozilla.org/zh-CN/docs/Web/Web_Components

### ✨ 划重点

首先: Web components 是一套标准，有不同的技术组成，允许我们编写模块化、可重用和封装的 HTML 元素。

它最大的**优点**:
由于它们是基于 web 标准的，我们不需要安装任何框架或库就可以开始使用它们。也就是可以通过普通的 javascript 编写 web 组件了！

### 📂 使用

作为开发者，我们都想要尽可能多的重用代码。这对于自定义**UI** 控件来说通常不是那么容易 — 想想复杂的 HTML（以及相关的样式和脚本），有时您不得不写很多代码来呈现自定义 **UI** 控件，在不依赖任何框架的情况下，想要复用自定义的**UI**控件只能不断的粘贴复制相关代码。

Web Components 旨在解决这些问题 — 它由**三项主要技术组成**，它们可以一起使用来创建封装特定功能的定制元素，可以在你喜欢的任何地方重用，不必担心代码或者样式冲突。

## 三项主要技术简单介绍：

### **1.Custom elements（自定义元素）**

一组 `JavaScript API`，允许您定义 `custom elements` 及其行为，然后可以在您的用户界面中按照需要使用它们。简单的解释就是，用户自定义 HTML 元素。它们通过使用`CustomElementRegistry`来注册一个新的元素，通过 window.customElements 中一个叫 define 的方法来获取注册的实例

```javascript
window.customElements.define('my-element', MyElement)
```

方法中的第一个参数定义了新创造元素的标签名字，我们可以非常简单的直接使用

```html
<my-element></my-element>
```

为了避免和 native 标签冲突，所以规定自定义标签强制使用中划线来连接。

### **2.Shadow DOM（影子 DOM）**

一组 `JavaScript API`，用于将封装的"影子"`DOM` 树附加到元素中（与主文档 `DOM` 分开呈现）并控制其关联的功能。通过这种方式，您可以保持元素的功能私有化，这样它们就可以被脚本化和样式化，而不用担心与文档的其他部分发生冲突。

`Web components` 的一个重要属性是**封装**——可以将**标记结构**、**样式**和**行为**隐藏起来，并与页面上的其他代码相隔离，保证不同的部分不会混在一起，可使代码更加干净、整洁。其中，`Shadow DOM` 接口是关键所在，它可以将一个隐藏的、独立的 DOM 附加到一个元素上。

使用 `Shadow DOM`，自定义元素的 `HTML` 和 `CSS` 完全封装在组件内。这意味着元素将以单个的 `HTML` 标签出现在文档的 `DOM` 树种。其内部的结构将会放在`#shadow-root`。
![image](https://user-images.githubusercontent.com/24740506/99316220-ec56fe00-289e-11eb-9360-d9bd9d50d0b0.png)

`Shadow DOM` 使得`HTML` 和 `CSS` 与**主文档**的`DOM`保持分离。

> 备注： Firefox（从版本 63 开始），Chrome，Opera 和 Safari 默认支持 Shadow DOM。基于 Chromium 的新 Edge 也支持 Shadow DOM；而旧 Edge 未能撑到支持此特性。

#### 为什么要把一些代码和网页上其他的代码分离？

原因之一是，大型站点若 `CSS` 没有良好的组织，导航的样式可能就『泄露』到本不应该去的地方，如主要内容区域，反之亦然。随着站点、应用的拓展，这样的事难以避免。

#### 重新认识

实际上一些原生的 HTML 元素也使用了 Shadow DOM。例如你再一个网页中有一个`<video>`元素，它将会作为一个单独的标签展示，但它也将显示播放和暂停视频的控件，当你在浏览器开发工具中查看 `video` 标签，是看不到这些控件。

这些控件实际上就是 `video` 元素的 `Shadow DOM` 的一部分，因此默认情况下是隐藏的。要在 `Chrome` 中显示 `Shadow DOM`，进入开发者工具中的 Preferences 中，选中 Show user agent Shadow DOM。当你在开发者工具中再次查看 video 元素时，你就可以看到该元素的 Shadow DOM 了。

**详细的操作步骤如下：**

![image](https://user-images.githubusercontent.com/24740506/99316960-2ffe3780-28a0-11eb-8602-ba0eca657779.png)

![image](https://user-images.githubusercontent.com/24740506/99317037-50c68d00-28a0-11eb-90d3-9345a8be1ebc.png)

![image](https://user-images.githubusercontent.com/24740506/99316814-f4fc0400-289f-11eb-89b8-60de8d4e4d8c.png)

##### 注意:

不管从哪个方面来看，`Shadow DOM` 都不是一个新事物——在过去的很长一段时间里，浏览器用它来封装一些元素的内部结构。以一个有着默认播放控制按钮的 `<video>` 元素为例。如上图所示，你所能看到的只是一个 `<video>` 标签，实际上，在它的 Shadow DOM 中，包含来一系列的按钮和其他控制器。Shadow DOM 标准允许你为你自己的元素（custom element）维护一组 Shadow DOM。

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

```js
let shadow = elementRef.attachShadow({ mode: 'open' })
let shadow = elementRef.attachShadow({ mode: 'closed' })
```

`open` 表示可以通过页面内的 `JavaScript` 方法来获取 `Shadow DOM`，例如使用 `Element.shadowRoot` 属性：

```js
let myShadowDom = myCustomElem.shadowRoot
```

如果你将一个 `Shadow root` 附加到一个 `Custom element` 上，并且将 `mode` 设置为 `closed`，那么就不可以从外部获取 `Shadow DOM` 了——`myCustomElem.shadowRoot` 将会返回 `null`。浏览器中的某些内置元素就是如此，例如`<video>`，包含了不可访问的 `Shadow DOM`

如果你想将一个 `Shadow DOM` 附加到 `custom element` 上，可以在 `custom element` 的构造函数中添加如下实现（目前，这是 `shadow DOM` 最实用的用法）：

```js
let shadow = this.attachShadow({ mode: 'open' })
```

将 `Shadow DOM` 附加到一个元素之后，就可以使用 `DOM APIs` 对它进行操作，就和处理常规 `DOM` 一样。

```js
var para = document.createElement('p')
shadow.appendChild(para)
```

### **3.HTML templates（HTML 模板）**

`<template>` 和 `<slot>`元素使您可以编写不在呈现页面中显示的标记模板。然后它们可以作为自定义元素结构的基础被多次重用。

除了使用 `this.shadowRoot.innerHTML` 来向一个元素的 `shadow root` 添加 `HTML`，你也可以使用 `<template>`来做。`template` 保存 `HTML` 供以后使用。它不会被渲染，并只有确保内容是有效的才会进行解析。模板中的 `JavaScript` 不会被执行，也会获取任何外部资源，默认情况下它是隐藏的。

当一个 `web component` 需要根据不同的情况来渲染不同的标记时，可以用不同的模板来完成

在[wheat-ui](https://github.com/glean-wheat/wheat-ui) 的 [button](https://github.com/glean-wheat/wheat-ui/blob/master/src/button/button.js#L372)组件 中 因为`button`组件会根据不同的属性渲染不同的标签

```js
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

## cloneNode

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

## 添加样式

自定义元素还没有样式，可以给它指定全局样式，比如下面这样。

```css
wheat-button {
  /* ... */
}
```

但是，组件的样式应该与代码封装在一起，只对自定义元素生效，不影响外部的全局样式。所以，可以把样式写在`<template>`里面。在[wheat-ui](https://github.com/glean-wheat/wheat-ui) 的 [button](https://github.com/glean-wheat/wheat-ui/blob/master/src/button/button.js#L372)组件;

```js
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

就像之前的 JQ 一样，当 document.querySelector 开始得到浏览器的广泛支持后，使得 JQ 的使用频率开始变低。这样的事也有可能发生在 Angular 、React、Vue 这些框架上

---

---

title: Web Components 基础入门-生命周期（中）

author: GleanWu
tags:

- WebComponent
- 组件化

---

中篇

# Web Components 生命周期

关于自定义元素的 生命周期

## ♻️ 组件的生命周期

让我们看一下自定义元素的生命周期，下面是一份代码备注，方便开始快速开发。

```js
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

> 看过上面的的代码注释后，下面关于生命周期的介绍是重复的；

## constructor() 初始化

`constructor()`方法是元素挂载之前运行的，一般会在这个函数中设置一些元素的**初始状态**，还有**事件的监听** 以及创建 `Shadow Dom`

> 类比 `Vue` 中的 `create()`，`React`中 `class` 组件中的 `constructor()`它们两个是一样的

## connectedCallback() 挂载完成

当元素插入到 DOM 中时，将调用 `connectedCallback`， 通常，组件的一些`样式`或者其他需要`获取元素的状态` 应该在 `connectedCallback` 进行设置，因为在这里才能确定元素的所有属性和子元素都是可用的。`constructor` 一般应该只用于初始化状态和设置`shadow DOM`。

### `constructor` 和 `connectedCallback` 之间的区别

创建元素时调用`constructor`，
已经插入到 `DOM` 元素后调用 `connectedCallback`

> 可以将其与`vue`的`mount`方法、`React`的`componentDidMount`方法进行比较

## disconnectedCallback 卸载元素

当`自定义元素`从`DOM`中移除的时候，就会触发该方法；同时也会在这个事件中删除调改元素添加的事件，比如全局的`键盘函数`以及`延时函数`，
disconnectedCallback 的对应函数是 connectedCallback，
另外有个注意点，当用户关闭浏览器或关闭该标签页时，将不会调用此方法。不过可以用 `window.unload beforeunload` 或者 `widow.close` 去触发在浏览器关闭是的回调

> `react` 中的 `componentWillUnmount` 的方法进行比较, `vue` 中的 `destory` 中是生命周期函数进行对比

## attributeChangedCallback 属性监听

使用这个函数需要和 `observedAttributes` 该方法配合使用，需要将你要监听的属性在该方法中注册；

```js
static get observedAttributes() {
    return ['my-attr']
}

```

上面的函数中，就是对`my-attr`属性进行了监听；当该属性的值发生改变的时候，就会触发`attributeChangedCallback`

```js
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

## 执行顺序

`constructor -> attributeChangedCallback -> connectedCallback`；vue 以及 react 中的生命周期也是这样的一个顺序；

这个时候，可能会有些困惑了。

#### 为什么 `attributeChangedCallback` 要在 `connectedCallback` 之前执行 ?

回想一下，web 组件上的属性主要用来初始化配置。这意味着当组件被插入`DOM`时，这些配置需要可以被访问了。因此`attributeChangedCallback`要在`connectedCallback`之前执行。 这意味着你需要根据某些属性的值，在`Shadow DOM`中配置任何节点，那么你需要在构造函数中引用这些节点，而不是在`connectedCallback`中引用它们。

例如，如果你有一个`ID`为`container`的组件，并且你需要在根据属性的改变来决定是否给这个元素添加一个灰色的背景，那么你可以在构造函数中引用这个元素，以便它可以在`attributeChangedCallback`中使用：

```js
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

## customels.define

另外，在自定义元素未注册时候使用它，它会仅仅作为 `HTMLUnknownElement`，一个实例；代表着一个无效的 HTML 元素，派生自 HTMLElement 接口,，但它没有任何可用的附加属性或方法

然后通过 `customements.define()` 注册时，它会继承`HTMLElement`这个类。这个过程称为升级。
通过 `customElements.whenDefined`方法，当自定义元素被定义时一个 `Promise` 返回`{jsxref("undefined")}}`. 如果自定义元素已经被定义，则 `resolve` 立即执行

```js

Promise<> customElements.whenDefined(name);

```

```js
customElements.whenDefined('my-element').then(() => {
  // my-element is now defined
})
```

## 浏览器的支持情况

Edge 在发布的 19 版本中开始提供支持。对于旧的浏览器，可以使用 [polyfill](https://github.com/webcomponents/webcomponentsjs) 来兼容 IE11

![image](https://user-images.githubusercontent.com/24740506/98744397-9856a000-23ec-11eb-9d63-80797ffaa36a.png)
