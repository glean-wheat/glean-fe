---
title: 跨项目区块复用方案实践
author: GleanWu
tags:
  - Bit
  - 区块
---

## 背景

在平时的前端业务开发中，常常需要使用一些组件库里的组件开发页面。然而单单这些组件一般很难完全满足业务需求，还需要针对不同的业务场景进行开发添加业务逻辑。当随着开发的前端项目数量越来越多，就会发现很多业务场景会经常遇到，而且基本大同小异，可能只需要修改少量的代码，原来开发的代码就可以在新的项目中使用。

例如账号绑定手机号这个场景，除了使用了 input、button 等组件等，还要添加很多例如校验手机号、设置倒计时、接口校验验证码等逻辑。如果输入验证码的样式比较特别，可能还会有基于通用 input 组件二次封装出专门针对验证码的输入框。当再次遇到类似绑定手机号的需求时，大部分前端往往会直接从原有的项目中拷贝一份到新的项目，然后做一些微调即可。

这方式可能会遇到以下几个问题：

- **可复用的业务场景代码散落在形形色色的前端业务项目中，信息不互通，跨项目搜索很难**。
  往往需要问些资历比较深的开发同事，才能知道某个业务场景在哪个项目中开发过。如果刚来的开发同事并不知道之前已经开发过，而是自己闷头从零开发，就会导致开发资源浪费的问题。
- **相似的业务场景在不同的业务项目里有着不同的代码实现，无法做到统一标准，共同实现一个最佳实践**。
  平时开发时经常会有这样一个问题：在不同的业务项目中都编写过相似的业务场景的代码，但都是不同人各自维护的，之间也没有过交流。就会导致后面新的项目开发相似的业务场景时，面临有多个版本代码的选择。无法做到共同维护一个版本代码，并不断优化和改造，最终实现在这个业务场景的最佳实践。

后面的内容就是笔者为了解决上述问题，而开发的跨项目区块复用平台的实践总结。讲到这里读者可能会有个疑问：什么是区块？为什么是区块复用而不是组件复用？

为了解答这个问题，我们先明确下这些概念的定义，下面直接引用阿里飞冰相关的定义：

组件（component）：功能比较确定同时复杂度较高，例如用户选择器、地址选择器等，项目中只需要引入对应的 npm 包即可，项目不关心也无法修改组件内部的代码，只能通过组件定义的 props 控制。

区块（block）：一般是一个 UI 模块，使用区块时会将区块代码拷贝到项目代码中，项目里可以对区块代码进行任何改动，因此区块后续的升级也不会对项目有任何影响，这是区块跟业务组件的最大区别。

对于组件，笔者所在公司有一个非常完善的流程了：将可复用的代码抽象成基础/业务组件，然后走 npm 包发布的流程，并展示在内建的组件平台上。使用者只需要在平台上找到自己需要的组件，然后通过私有 npm 源下载到项目的依赖中即可使用。

而对于区块，一般很难抽象成组件并集成到 npm 包里，使用时往往需要直接修改区块的源码。而针对区块的复用，目前并没有合适的工具可以使用，所以才会主要针对区块实现了一个可共享复用的平台。特别说明一下，本文的区块除了包括 UI 相关的代码，也包括一些可复用的 utils 方法等等。

这个平台是基于 [Bit](https://github.com/teambit/bit) 开发的，所以在阐述区块复用平台的实现之前，需要先介绍下 Bit 的原理。

## Bit 基本原理

为了避免读者的困扰，这里先提前声明一下，在这个小节里会经常出现 `组件` 这一词，读者可以理解成 `Bit 组件`--即可复用的代码片段。原因是 Bit 本身并没有区分组件和区块，凡是可复用的代码片段都可以通过 Bit 来实现复用，只是笔者主要用它来实现区块共享而已。下面是 Bit 的原理图：

![Bit 原理图.png](https://camo.githubusercontent.com/55572ce94daee1f3683a29ae01f2f3b62ac65889/68747470733a2f2f692e6c6f6c692e6e65742f323032302f30392f31322f7432736e4850784d6c4236557076592e706e67)

Bit 是一个用于跨项目组件协作的开源 CLI 工具。使用 Bit 将分散在各个项目中的组件转化可复用的 Package，并可以跨项目使用。

你可以设置自己的用于组件协作的服务，也可以使用 [Bit.dev cloud](https://Bit.dev/) 托管组件，用于私有或共有组件的共享。

上面是 Bit 官方文档对 Bit 的定义，读者可能会觉得和 Git 有点相似。Bit 的确在实现中受到 Git 很大启发。不过区别在于 Git 是以文件为维度的，而 Bit 是以组件为维度。想了解更多内容可以点击 [Bit Docs](https://docs.Bit.dev/docs/quick-start) 。

### Bit 组件的定义和要素

关于上面定义中的提到 Bit 组件，Bit 也给出了自己的定义：

- **一个 React, Vue or Angular 组件**
- **公共样式文件 (例如 CSS, SCSS)**
- **可复用的方法**

针对每个组件 Bit 主要存储以下三个要素：

- **源代码（包括代码、测试和文档）**
- **依赖图谱**
  当添加文件到 Bit 组件时，Bit 会分析该文件的引入的依赖（例如代码中的 import 或 require 语句）。依赖图谱使组件独立于项目存在，可以跨项目移动且不丢失任何引用。
  需要注意的是，这里追踪的依赖项只包含使用 NPM 安装的依赖和安装的 Bit 组件。也就是说项目中直接引入的本地文件不被包含在依赖项内，例如 `import { computeXXX } from '../utils'`。不过不必担心，当在本地执行发布组件到远程的命令时，Bit 会检测引入的本地文件是否也被追踪，没有的话是无法发布的。
- **工具和配置**
  Bit 还会将组件特有的工具和配置保存下来，比如组件使用的编译器和测试工具等。

下面这张图生动的呈现了一个 Bit 组件的构成要素。

![Bit-component.png](https://camo.githubusercontent.com/9c2e6ccc97cc91dc677ef068615d6f3b54d1a1e3/68747470733a2f2f692e6c6f6c692e6e65742f323032302f30392f32372f5a6244487838316e4b494a477a516a2e706e67)

### Bit 组件的生命周期

Bit 组件的发布和使用都是通过开源的 CLI 工具 [bit-bin](https://www.npmjs.com/package/bit-bin) 来实现的，读者可以在自己的电脑上全局安装这个 npm 包，尝试用它发布个组件体验下。

#### 发布组件到远程仓库

- **Track**: 通过指定组成组件的文件，来初始化一个 Bit 组件。同时这些文件的内容修改会被追踪。具体命令：`bit add src/bindPhone/xxx -i bindPhone`。
- **Version**: 给组件标记版本，会将这个版本的组件的元数据和文件内容固化下来。具体命令：`bit tag bindPhone 1.0.0`。
- **Export**: 导出组件会为组件创建一个唯一的 ID。这个唯一 ID 包含了 Remote Scope 和本地组件名称。export 指令会将组件的元数据和文件内容的拷贝推送到远程仓库。具体命令：`bit export [remoteScopeName]`。

#### 使用组件

当组件被推送到服务器上的远程仓库，其他本地的 Bit WorkSpace 就可以使用这个组件了。使用的方式包括了两种：一种是 Install 方式--将组件作为一个常规的 npm 包安装到 node_modules 中，另一种方式是 Import 方式--将组件的源代码以及依赖等信息下载到本地。

读者可以再结合下面这张图来理解上面 Bit 组件生命周期的内容。

![Bit-CLI.png](https://camo.githubusercontent.com/56db808214a9b3e88ffe618e132bed56c8e0d3ab/68747470733a2f2f692e6c6f6c692e6e65742f323032302f30392f32372f35715554504144534a4e61496243562e706e67)

### Bit 部分概念解释

#### Workspace（工作区）

当在前端业务项目中执行 `bit init` 命令时，整个业务项目就变成了 workspace（工作区），类似 Git 中的工作区概念。

#### Scope（仓库）

当在前端业务项目中执行 `bit init` 命令时，会生成一个 `.bit`目录，这个目录就是 bit scope（仓库），类似 Git 的 .git 目录就是 git repository（仓库）。

一个 scope 可以存在或者不存在 Bit 工作区中，组件通过 `bit export` 和 `bit import` 命令在不同的 scope 之间传递，另外也可以使用 `bit tag` 和 `bit checkout` 命令将单个版本的组件从本地 scope（仓库） 和本地 workspace（工作区） 之间进行转换。

组件在 scope 中是采用 CAS(content addressable storage 内容寻址存储) 存储的，关于 scope 的存储的原理后面会详细阐述。Bit 受到了 Git 的机制很大的启发，如果读者对 Git 熟悉的话，就会更容易理解 Bit。

#### Remote Scope（远程仓库）

Remote scope 是保存在服务器上的，也可以叫 bare scope，因为这个 scope（仓库） 是在 workspace（工作区） 之外的。Remote scope 是主要是用于共享组件的，也就是组件的导出/导入的地方。

## 实现跨项目区块复用方案

通过上面的介绍，相信读者对 Bit 已经有了初步认知，其实笔者认为 Bit 非常适合跨项目区块复用平台的最主要的原因在于：发布者无需类似发布 npm 一样，需要单独创建项目并发布，而是**可以直接在业务项目中发布可复用的区块代码**。这一点非常适用区块的**很难抽象且代码在项目中可以任意改动的特点**。

那么剩下需要思考的问题就是如何在 Bit 基础上实现整个跨项目区块复用平台方案。下面这张图是整个方案的架构图，下面的小节会针对架构图中的不同部分分别做阐述。

![区块复用平台基本原理](https://camo.githubusercontent.com/4dd72bba83b25a142a077ed374123b6be74e2732/68747470733a2f2f692e6c6f6c692e6e65742f323032302f30392f31332f56754155764e777833533166486f5a2e706e67)

### Bit 远程仓库（Bit Remote Scope）

Bit 官方已经提供了在服务器上部署远程仓库的方案，可以在远程服务器上执行 Bit 的`bit init --bare` 命令初始化一个远程仓库，或者直接部署 Bit 官方提供的 Docker 镜像 [bit-docker](https://github.com/teambit/bit-docker)。

部署完远程仓库后，使用者就可以通过 ssh 协议将本地仓库的区块代码上传到远程仓库中，或者从远程仓库中下载区块代码。

更多细节可以参考官方文档 [bit-server](https://docs.bit.dev/docs/bit-server)。

### Bit CLI

上个小节中提到的上传和下载区块代码的操作都是通过 Bit 开源的 CLI 工具 [bit-bin](https://www.npmjs.com/package/bit-bin) 实现的，读者可以直接在实际开发中使用。

不过如果有一些特定的需求，例如在执行 `bit import` 下载区块代码时，需要记录下载次数到区块平台中等，就需要定制 [bit-bin](https://www.npmjs.com/package/bit-bin)。对此笔者建议直接克隆一份 Bit 源码 [bit](https://github.com/teambit/bit)，然后进行二次定制开发，并通过在公司内部发布私有 npm 包的方式提供开发使用。

### 区块平台

经过上面的操作，区块代码已经托管在服务器上的远程仓库（Remote Scope）中，但区块使用者还无法很直观地通过查看区块代码构建出来的视图来选择区块，也无法对区块代码进行在线调试查看效果，这对区块的使用造成了很大困扰。

而官方提供的门户站点 [bit.dev](https://bit.dev/) 虽然有这些功能但并没有开源，所以我们需要做一个类似功能的站点。通过分析 [bit.dev](https://bit.dev/) 站点的功能，可以发现站点实现中的两个关键点：

1. 实时构建区块代码，然后对构建出的页面截图，展示在区块列表中。并且可以在线调试区块源码，然后实时看到调试后的构建结果；
2. 从远程仓库存储的文件中解析出某个区块的数据（源码、依赖等等），以便在区块平台中使用。

关于第一点，主要需要一个在线 IDE 的支持，对此笔者之前已经总结了一篇文章--[搭建一个属于自己的在线 IDE](https://github.com/mcuking/blog/issues/86)，这里就不再赘述了。接下来主要阐述下第二点的实现。

#### 从远程仓库中解析区块数据

还记得之前有提到 Bit 的 Scope（仓库） 是采用 CAS（content addressable storage 内容寻址存储） 存储 Bit 组件的文件吗？接下来我们就详细的介绍其中的原理。

经过对 Bit 源码的分析，我们发现 Bit 组件的文件存储和 Git 非常相似，所以首先了解下 Git 是怎么做文件存储的，这里主要参考了文章 [Git 内部存储原理](https://zhaohuabing.com/post/2019-01-21-git/) 的内容：

Git 的本质是一个文件系统，其工作空间中的所有文件的历史版本以及提交记录(Commit)、branch、tag 等信息都是以文件对象的方式保存在 .git 目录中的。在 .git 下的 objects 目录下可能会看下面这类文件：

```javascript
.git/objects
├── 06
│   └── 5bcad11008c5e958ff743f2445551e05561f59
├── 3b
│   └── 18e512dba79e4c8300dd08aeb37f8e728b8dad
├── info
└── pack
```

Git Objects 目录中的文件类型主要有以下三种：

- **Commit**: Commit 对象，记录了一个 Version 的所有目录和文件信息
- **Tree**: 目录对象，记录了该目录下包含那些目录和文件信息
- **Blob**: 文件对象，记录了文件内容

而 Git Objects 是通过下面的方式处理并存储在 Git 内部的文件系统中的：

1. 首先创建一个 header，header 的值为 "对象类型 内容长度\0"；
2. 将 header 和文件内容连接起来，计算得到其 SHA-1 hash 值（40 个十六进制的数字组成的字符串）；
3. 将连接得到的内容采用 zlib 压缩；
4. 将压缩后的内容写入到以 "hash 值前两位命令的目录/hash 值后 38 位命令的文件" 中。

在 Bit 源码中， Bit Scope 中的 objects 文件也分成以下几种类型：

- **Component**: 记录了 Bit 组件的相关信息，包括区块名称、历史版本等
- **Version**: 记录了每次发布的版本信息，例如这次版本的包含的文件、依赖、发布者邮箱/用户名、发布时间等
- **Source**: 记录了文件内容
- **Symlink**: 暂时无用
- **Scope**: 暂时无用

而 Bit Objects 在处理和存储上面这些信息的方式也和 Git 大同小异：

1. 首先根据文件内容计算得到其 SHA-1 hash 值（40 个十六进制的数字组成的字符串）；
2. 然后创建一个 header，header 的值为 "对象类型 文件内容的 SHA-1 hash 值 内容长度\0"；
3. 将 header 和文件内容连接起来；
4. 将连接得到的内容采用 zlib 压缩；
5. 将压缩后的内容写入到以 "hash 值前两位命令的目录/hash 值后 38 位命令的文件" 中。

区别在于两点：一个是 Git 是根据 `header + 文件内容` 两者相加组成的完整内容计算的 SHA-1 hash 值，而 Bit 仅仅根据文件内容计算 SHA-1 hash 值；另一个点是 Bit 的 header 中还额外包括文件内容的 SHA-1 hash 值。

既然我们知道了数据是如何被处理和存储成这些文件，那么就可以反过来从这些文件中解析出这些数据，下面就是解析文件的方法：

```javascript
const zlib = require('zlib');
const fs = require('fs-extra');

const SPACE_DELIMITER = ' ';

const NULL_BYTE = '\u0000';

const inflate = (buffer) ={
    return new Promise((resolve, reject) ={
        zlib.inflate(buffer, (err, res) ={
            if (err) return reject(err);
            return resolve(res);
        });
    });
}

// 将对象转化成 buffer    const buf = Buffer.from(JSON.stringify(obj));
// 将 buffer 转化成对象   const temp = JSON.parse(buf.toString());
const parse = (buffer) ={
    // 使用分隔符号 '\u0000' 将文件内容分成 header 和 content 两部分

    const firstNullByteLocation = buffer.indexOf(NULL_BYTE);
    // 头部部分
    const headers = buffer.slice(0, firstNullByteLocation).toString();
    // 内容部分
    const contents = buffer.slice(firstNullByteLocation + 1, buffer.length);

    const [type] = headers.split(SPACE_DELIMITER);

    console.log('file type is:', headers);

    if (type === 'Source') {
        return contents.toString();
    }

    return JSON.parse(contents.toString());
}

const parseObject = async (path) ={
    const contents = await fs
        .readFile(path)
        .then(fileContents ={
            return inflate(fileContents);
        })
        .then(buffer =parse(buffer));

    console.log('file contents is:', contents);
    return contents;
}

parseObject('/Users/xxx/bit/common/objects/03/3cb8b37245cf0cfbde2495d5d88c1324234e96');
```

然后就可以调用 parseObject 方法去解析不同类型文件的内容，例如 Component 文件的示例内容如下：

```javascript
{
  name: 'button',
  scope: 'common',
  versions: {
    '1.0.0': '4873cd3d4efdd585ee9a960bdfb16f2ee986ab14',
    '1.0.1': 'e1e8280f56c5bfca8640e186f5667286b2023927'
  },
  lang: 'javascript',
  deprecated: false,
  bindingPrefix: '@bit',
  remotes: [
    {
      url: 'file:///Users/xxx/bit/common',
      name: 'common',
      date: '1599218799176'
    }
}
```

Version 文件示例内容如下：

```javascript
{
  files: [
    {
      file: '0b8b28f212101ef236744a25bfa085a00d0e7a63',
      relativePath: 'src/components/button/index.js',
      name: 'index.js',
      test: false
    }
  ],
  mainFile: 'src/components/button/index.js',
  bindingPrefix: '@bit',
  log: {
    message: '',
    date: '1599218793164',
    username: 'xxx',
    email: 'xxx@xxx.com'
  },
  ci: {},
  docs: [],
  dependencies: [],
  devDependencies: [],
  flattenedDependencies: [],
  flattenedDevDependencies: [],
  extensions: [],
  packageDependencies: { react: '^16.13.1' },
  devPackageDependencies: {},
  ...
}
```

Source 文件内容其实就是区块的源码，这里就不展示了。

接下来的分析中又发现本地 scope 中（即 .bit 目录中）的 index.json 文件中记录了 Bit 组件的对应的 Component 文件的 SHA-1 hash 值。如下所示：

```javascript
;[
  {
    id: {
      scope: 'common',
      name: 'button'
    },
    isSymlink: false,
    hash: '2179ca06272f0962fafd793abdf27a553fd9b418' // 对应组件的 Component 文件
  }
]
```

根据以上分析到的知识点，我们就可以找出从远程仓库 Scope 的 Objects 中解析出我们需要的区块源代码的方法了，大致步骤如下：

1. 首先从 scope 中的 index.json 中找到对应区块名称，并获取到区块对应的 Component 文件的 hash 值；
2. 使用上面的 parseObject 方法解析出 Component 文件的内容，并从 Component 文件内容中的 versions 字段找到区块最新版本对应的 Version 文件的 hash 值；
3. 使用上面的 parseObject 方法解析出 Version 文件的内容，从 Version 文件内容中的 files 字段就可以找到该区块包含的所有源码文件名称、相对路径、hash 值等，从 dependencies、devDependencies 等字段中就可以获取区块所有的依赖；
4. 将上个步骤中获取到的区块源代码/依赖等数据，按照一定的格式返回给区块平台即可。

这样就达到了从 Bit 远程仓库中解析出某个区块的源码和依赖等数据，并返回给区块平台的目的。由于篇幅有限，具体代码就不在这里展示了。

到此整个架构的实践就已将介绍完了。当然在这个基础上还可以做很多有趣的事情，例如编写一个 VSCode 插件用于在编辑器右侧展示区块平台上的所有区块，用户可以搜索浏览区块，点击区块即可下载到项目中，并自动引入到代码里。效果如下图所示：

![区块 VSCode 插件](https://camo.githubusercontent.com/2e72218162b41cd66cf45e328c961d8efd77ce09/68747470733a2f2f692e6c6f6c692e6e65742f323032302f30392f32372f5857767449426f52616a78413754702e706e67)

## 结束语

如果做个类比的话，区块复用平台就像冶金设备，而前端的业务项目就像一座座矿山，区块复用平台的使命就是从这么多前端项目中冶炼出有复用价值的金子--区块，并将这些金子直观地展示给开发者，使其尽可能复用这些区块，以提升开发效率。

## 参考资料

-[飞冰-关于物料](https://ice.work/docs/materials/about)

-[Git 内部存储原理](https://zhaohuabing.com/post/2019-01-21-git/)
